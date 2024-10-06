
**atom.js**
- {defaultList}
- atomMapKey
- atomMap

**atomMap.js**
- jotaiHydrator

**index.page.jsx**
- useAtomValue
- getServerSideProps return initialData key값

**\_app.page.jsx**
- App함수 파라미터로 data를 받아서 콘솔에 찍어보면
  1) index.page.jsx의 Page가 Component에 들어간다.
  2) index.page.jsx의 getServerSideProps에서 리턴되는 props가 pageProps에 들어간다.
  3) 그 외에도 pathName, query 외에도 다른 정보도 들어온다.
index.page.jsx에서 getServerSideProps에서 파라미터로 받는 data와는 다른 놈이다.
[next.js pages Router API](https://nextjs.org/docs/pages/building-your-application/rendering)


**atom이 흘러가는 플로우**
1. index.page.jsx 에서 getServerSideProps에서 백서버에 API 호출하여 값을 받아 props의 initialData로 atomMapKey에 넣어 리턴한다.
2. \_app.page.jsx에서 pageProps로 받아 jotaiHydrator에 initialData(구조분해할당)를 넘긴다.
   ``` js
initialData >>> {
  notice__noticesAtom: { // atom.js에서 정의한 key값
    total: 28,
    count: 0,
    limit: 15,
    offset: 0,
    items: [
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object], [Object],
      [Object]
    ]
  }
}
```
4. jotaiHydrator가 실행되면 아래의 배열안에 배열로 리턴된다.(atomMap.js 안에 실제 코드가 있음)
   ``` js
[
	[noticesAtom, 서버에서 호출된 noticies 값]
]
```
5. 다시 \_app.page.jsx에서 위의 결과값이 hydrates의 변수에 담겨서 JotaiProvider로 넘긴다. 
6. jotai에서 제공하는 useHydrateAtoms를 이용해서 atom에 서버에서 호출된 noticies 값을 넣어준다. (https://jotai.org/docs/utilities/ssr#usehydrateatoms)
8. 다시 index.page.jsx의 Page에서 useAtomValue(noticesAtom)을 호출해보면 atom 초기값(defaultList)이 서버에서 호출된 값으로 바뀌어있다!
