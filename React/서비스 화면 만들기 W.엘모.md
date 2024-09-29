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


### 2) 


