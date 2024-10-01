
CSR에서 API 호출하는 방법은 두가지가 있다.
- [0] 방법1) 버튼이나 사용자의 액션을 통해서 API와 통신 
- [1] 방법2) 페이지가 처음 렌더링된 이후 실행되는 API → `useEffect`

그  중 두번째 방법인 `useEffect`로 API를 CSR에서 호출해 본다.

페이지가 처음 렌더링될 때 CSR에서 호출하는 API는 `useEffect()` 안에서 실행된다.
# 1. 데이터를 가져오는 함수를 하나 만들어야 한다.

**src/pages/services/\[type\]/`index.page.jsx`**
``` jsx
import { useGetRecoilValueInfo_UNSTABLE, useRecoilValue } from 'recoil';

import { servicesAtom } from '@modules/service/atom';
import { findServices } from '@modules/service/fetch';

// 서버에서 실행되는 코드 
export const getServerSideProps = async data => {
	...
}

// 클라이언트에서 실행되는 코드
const Page = () => {
	const getProgramData = async () => {
		const response = await findServices();
	}

	return <div>서비스 페이지</div>;
}
```

## 1) `async` 
데이터를 처리하는 과정에서 두 가지 방식이 있는데, 동기와 비동기이다.
이 둘의 큰 차이를 단번에 쉽게 이해하기 위해 나의 프런트 스승님께서 들려주신 예시를 가져왔다.

- [3] 동기
화장실에서 큰일을 보고 있다 → 동생한테 휴지 좀 가져와 요청을 한다 → 변기 앉은 채로 휴지가 올 때까지 기다린다
- [4] 비동기
화장실에서 큰일을 보고 있다, 휴지가 없다 → 휴지 좀 가져와 요청을 한다 → 화장실 밖으로 나가 티비를 본다 → 밥도 먹는다 → 휴지가 도착했다 → 닦는다

이렇게 동기와 비동기의 차이는 서버와 API 통신 중 다른 과정을 수행할 수 있느냐 없느냐의 차이인데,
그동안 스피너, 마우스 무빙, 타이핑 모두 서버의 resp가 올 때까지 기다리는 동안 발생하는 이벤트이므로 비동기이다.

getProgramData에 async가 붙는 이유는 비동기 통신을 위해서이다.

## 2) `await`

fetch 함수에 `await`을 사용하는 이뉴는 비동기 요청 시, 순서를 보장하기 위함이다.
# 2. useEffect 사용하기

useEffect는 브라우저에서 렌더링된 이후 시점에 실행되기 때문에 서버에서 실행할 수 없어 CSR에서 데이터를 불러오기 위해 사용되는 훅이다.

**src/pages/services/\[type\]/`index.page.jsx`**
``` jsx
...

// 클라이언트에서 실행되는 코드
const Page = () => {
	const getProgramData = async () => {
		const response = await findServices(); // 비동기 함수의 결과값이 atom 값에 들어감 === atom 값이 바뀐다. === 리렌더링
	}
	
	useEffect(() => {
		getProgramData(); // 리랜더링될 때마다 서버 API를 호출하지 않기 위해 의존성배열을 빈 배열로 한다.
	}, [])
	
	return <div>서비스 페이지</div>;
}
```

## 1) 의존성 배열

의존성 배열: 어떤 값이 바뀌었을 때, `useEffect`를 다시 실행되게 하는 배열

`useEffect`의 두번째 파라미터에는 의존성 배열을 넣을 수 있는데, 3가지 방법이 있다.
### (1) 빈 배열
처음 렌더링 이후 딱 한 번만 실행된다.
``` jsx
useEffect(() => {
	console.log('useEffect 안에서 실행되었습니닷!!);
}, [])
```

### (2) \[어떤값]
어떤값이 바뀔 때마다 `useEffect`가 실행된다.
``` jsx
useEffect(() => {
	console.log('useEffect 안에서 실행되었습니닷!!);
}, [어떤값])
```

### (3) 아무것도 안 넣기
렌더링될 때마다 계속 실행된다.
``` jsx
useEffect(() => {
	console.log('useEffect 안에서 실행되었습니닷!!);
})
```


> **SEO(Search Engine Optimization)과 SSR, CSR**
> 
> 브라우저에서 렌더링된 이후에 useEffect가 실행되기 때문에 서버에서 실행할 수 없다. CSR에서 데이터는 useEffect가 불러오는데, 크롤링 챗봇은 SSR에서 불러와진 데이터를 크롤링하기 때문에 CSR에서 useEffect로 불러온 데이터가 없어 CSR에서 호출한 데이터는 가져가지지 않는다.
