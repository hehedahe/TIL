## 1. 레이아웃 만들기

```
src
ㄴ pages
	ㄴ api/v1
		ㄴ `index.page.jsx`
	ㄴ services
		ㄴ `index.page.jsx`
	ㄴ `_app.pages.jsx`
ㄴ modules
	ㄴ services
		ㄴ `atom.js`
		ㄴ `fetch.js`
ㄴ system
	ㄴ fetcher
		ㄴ `fetch.js`
`next.config.js`
```

### 1) atom
> src/modules/srvivces/`atom.js`
> src/modules/`atomMap.js` → `atom.js`에서 정의한 `key`와 atom을 추가한다.

**atom.js**
``` js
import { atom } from 'recoil';

export const servicesAtom = atom({
  key: 'service__servicesAtom',
  default: {
    offset: 0,
    limit: 0,
    count: 0,
    total: 0,
    items: [],
  },
});
```
**atomMap.js**
``` js
import { servicesAtom } from './service/atom';

const atomMap = Object.freeze({
	...
	[servicesAtom.key]: servicesAtom, // 위에서 설정한 'service__servicesAtom'이 atomMap의 키가 되고, service/fetch.js에서 호출한 API 리턴값이 value로 들어간다.
});
  
export const recoilHydrator = (initialData, setter) => {
	// 초기 Recoil 상태 설정
	Object.keys(initialData).forEach(key => {
		setter(atomMap[key], initialData[key]);
	});
};
```


### 2) 목없API
> src/pages/api/v1/services/`index.api.js`

faker 
``` js
import { faker } from '@faker-js/faker';

export default function handler(req, res) {
  ...
  const getItem = index => {
    // 실제 뿌려지는 데이터
	return {
      id: faker.string.alpha(12),
      type: faker.helpers.arrayElement([
        'NURSING',
        'RECOGNITION',
        'RECUPERATION',
        'LEISURE',
        'EMOTION',
        'FAMILY',
      ]),
      title: faker.word.noun(),
      content: faker.commerce.productDescription(),
      regDate: faker.date.anytime(),
      order: index + 1,
    };
  };
  
  switch (req.method) {
    case 'GET':
      return res.status(200).json({
        data: {
          total,
          count: 0,
          limit: nLimit,
          offset: nOffset,
          items: Array.from(
            Array(total - nOffset > nLimit ? nLimit : total - nOffset),
          ).map((_, index) => getItem(index)),
        },
      });

    default:
      throw new Error('no supported method');
  }
}

```

### 3) Page
> /src/pages/services/`index.page.jsx`

#### (1) index.page.jsx의 기본 와꾸를 만든다.
``` jsx
// 서버에서 실행되는 코드
export const getServerSideProps = async () => {
	return {
		props: {},
	};
};

// 클라이언트에서 실행되는 코드
const Page = () => {
	return <div>서비스 페이지</div>;
};

export default Page;
```

#### (2) SSR에서 API 호출해본다.

``` jsx
// 서버에서 실행되는 코드
export const getServerSideProps = async () => {
	// 0. data를 fetch한다.
	// getServerSideProps의 첫 번째 파라미터는 data이다.
	// 브라우저의 토큰 정보나 호출한 URL, 쿼리 등 다양한 정보가 들어있다.
	const { query } = data;
	// console.log('data >>>>> ', data);
	console.log('query >>>>> ', query);
	
	// 1. API를 호출해서 리스폰스를 받는다.
	// 2. 리스폰스를 atom에 hydration 시켜준다.
	//    hydration: 서버에서 받은 response를 atom에 넣어주는 것
	return {
	    props: {
	      initialData: {
	        [servicesAtom.key]: services,
	      },
	    },
	};
};

// 클라이언트에서 실행되는 코드
const Page = () => {
	// 3. atom에서 값 꺼내오기
	const services = useRecoilValue(servicesAtom);
	console.log('services >>>>> ', services);
	return <div>서비스 페이지</div>;
};

export default Page;
```
참고: [[Next.js hydration]] 


### 4) 서비스 항목별 페이지
제공하는 서비스가 여러 종류가 있고, 각 서비스 페이지를 똑같은 화면으로 만들어야 한다.
그러면 이렇게 services 폴더 아래에 각 서비스 유형 폴더가 있고, 그 안에 실제 화면에 뿌려지는 `index.page.jsx` 파일이 있다.

#### (1) 서비스 항목별 폴더 구분하기
`getServerSideProps` 에서 fetch할 때 path로 각 서비스 화면에 일치하는 서비스 유형`type`을 보내고,
Page컴포넌트에서는 이 화면이 어떤 서비스 유형 화면인지 `<div>`에 나타낸다.

**`index.page.jsx`**
``` jsx
import { useRecoilValue } from 'recoil';

import { servicesAtom } from '@modules/service/atom';
import { findServices } from '@modules/service/fetch';

// 서버에서 실행되는 코드
export const getServerSideProps = async data => {
  const { query } = data;

  // ... -> Spread 문법을 사용하여 query안의 페이징 정보와 다른 쿼리를 같이 보낸다.
  // {} 안에 query를 그냥 사용하게 되면, query: {} 형태로 객체 자체가 value로 들어간다.
  const services = await findServices({ ...query, type: 'NURSING' });

  return {
    props: {
      initialData: {
        [servicesAtom.key]: services,
      },
    },
  };
};

// 클라이언트에서 실행되는 코드
const Page = () => {
  const services = useRecoilValue(servicesAtom);

  return <div>NURSING 페이지</div>;
};

export default Page;

```

#### (2) 폴더 하나, 파일 하나로 관리하기

서비스 화면은 `pages`폴더 아래 경로에 있는 `index.page.jsx`로 렌더링되기 때문에 위에서는 서비스 유형마다 각각 서로 다른 폴더와 파일로 구분해서 생성했다.
유저들에게 보여지는 url은 `/services/nursing`, `services/recognition`, ... 등으로 보여진다.
그런데 같은 화면과 같은 기능을 하는 페이지이고 페이지마다 유형, 즉 path만 달라지기 때문에 이런 경우, 한 폴더와 `index.page.jsx`로 관리할 수 있다.

**AS-IS**
```
src
ㄴ pages
	ㄴ services
		ㄴ nursing
			ㄴ `index.page.jsx`
		ㄴ recognition
			ㄴ `index.page.jsx`
		ㄴ recuperation
			ㄴ `index.page.jsx`
		ㄴ leisure
			ㄴ `index.page.jsx`
		ㄴ emotion
			ㄴ `index.page.jsx`
		ㄴ family
			ㄴ `index.page.jsx`
	ㄴ `_app.pages.jsx`
```

서비스 유형이 `pathVariable`로 경로가 나뉘어지는 경우, 폴더명을 대괄호로 감싸면 위에서 여러개의 폴더와 파일로 관리되던 페이지가 아래 구조처럼  `[type]` 하나로 간단해진다.

이는 **next.js**에서 제공하는 `dynamic routes` 를 이용하면 위에 설명한 것처럼 pathVariable로 입력한 값이 getServerSideProps의 디폴트 파라미터인 `data`의 `query`에 담긴다.

**TO-BE**
```
src
ㄴ pages
	ㄴ services
		ㄴ [type]
			ㄴ `index.page.jsx`
	ㄴ `_app.pages.jsx`
```

**`index.page.jsx`**
``` jsx
import { useRecoilValue } from 'recoil';

import { servicesAtom } from '@modules/service/atom';
import { findServices } from '@modules/service/fetch';

// 서버에서 실행되는 코드
export const getServerSideProps = async data => {
  const { query } = data;
  console.log('query >>>>> ', query);

  const { type } = query;
  console.log('type >>>>> ', type);
  const services = await findServices({ ...query });

  return {
    props: {
      title: '서비스',
      type: type,
      initialData: {
        [servicesAtom.key]: services,
      },
    },
  };
};

// 클라이언트에서 실행되는 코드
const Page = ({ type }) => {
  const services = useRecoilValue(servicesAtom);
  console.log('services >>>>> ', services);

  // type UI 분기처리
  return (<div>{type} 페이지</div>
  );
};

export default Page;
```

참고
- [5] dynamic route
https://nextjs.org/docs/pages/building-your-application/routing/dynamic-routes
