
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
그동안 스피너, 마우스 움직이는 것, 타이핑 가능한 것 모두 서버의 resp가 올 때까지 기다리는 동안 발생하는 이벤트이므로 비동기이다.
// async 붙는 이유? async === 비동기
# 2. useEffect 사용하기
``` jsx
...

// 클라이언트에서 실행되는 코드
const Page = () => {
	const getProgramData = async () => {
		const response = await findServices();
	}
	
	useEffect(() => {
		getProgramData();
	}, [])
	
	return <div>서비스 페이지</div>;
}
```


``` jsx

// 클라이언트에서 실행되는 코드
const Page = () => {
  // useEffect 두번째 파마리터인 의존성 배열을 넣는 방법 3가지
  // 의존성 배열: 어떤 값이 바뀌었을 때, useEffect를 다시 실행되게 하는 배열!
  // 1) [] -> 처음 렌디링 이후 한번만 실행된다.
  // useEffect(() => {
  //   console.log('useEffect 안에서 실행되었습니다!!');
  // }, []);
  // 2) [어떤값] -> 어떤값이 바뀔 때마다 useEffect가 실행된다.
  // useEffect(() => {
  //   console.log('useEffect 안에서 실행되었습니다!!');
  // }, [어떤값]);
  // 3) 아무것도 안 넣기 -> 이러면 렌더링될 때마다 계속 실행된다.
  // useEffect(() => {
  //   console.log('useEffect 안에서 실행되었습니다!!');
  // });

  const getProgramData = async () => {
    const response = await fineServies(); // 비동기 함수의 결과값이 atom 값에 들어감 === atom 값이 바뀐다. === 리렌더링
    // await 사용 이유?
    // => async 비동기 요청시, 순서를 보장하기 위함이다.
  };

  useEffect(() => {
    getProgramData();
  }, []); // 리랜더링될 때마다 서버 API를 호출하지 않기 위해 의존성배열을 빈 배열로 한다.

  return <div>서비스 페이지</div>;
};

export default Page;

// useEffect -> 브라우저에서 렌더링된 이후에 useEffect가 실행되기 때문에 서버에서 실행할 수 없어
// 데이터는 useEffect가 불러오는데, 데이터가 없어 크롤링 챗봇이 데이터를 얻어갈 수 없다.

```