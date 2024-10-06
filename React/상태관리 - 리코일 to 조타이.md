
atom.js
- {defaultList}
- atomMapKey
- atomMap

atomMap.js
- jotaiHydrator
	- 

index.page.jsx
- useAtomValue
- getServerSideProps return initialData key값

\_app.page.jsx
- App함수 파라미터로 data를 받아서 콘솔에 찍어보면
  1) index.page.jsx의 Page가 Component에 들어간다.
  2) index.page.jsx의 getServerSideProps에서 리턴되는 props가 pageProps에 들어간다.
  3) 그 외에도 pathName, query 외에도 다른 정보도 들어온다.
index.page.jsx에서 getServerSideProps에서 파라미터로 받는 data와는 다른 놈이다.
[next.js pages Router API](https://nextjs.org/docs/pages/building-your-application/rendering)


