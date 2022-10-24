# 𝙍𝙀𝙎𝙏 𝘼𝙋𝙄
`Representational State Transfer API`

- 2000년도에 Roy Fielding의 박사학위 논문에서 최초로 소개되었다.
- 로이 필딩은 HTTP 주요 저자 중 한 사람으로 웹(Http)의 장점을 최대한 활용할 수 있는 아키텍처로써 Rest를 발표했다.


# 𝙍𝙀𝙎𝙏 구성
- 자원(Resource) `URI`
- 행위(Verb) `Http Method`
- 표현(Representations)


# 𝙍𝙀𝙎𝙏 특징
- Uniform Interface
- Stateless
- Cacheable
- Self-descriptiveness
- Client - server 구조
- 계층형 구조


# 𝙍𝙀𝙎𝙏 𝘼𝙋𝙄 설계
- URI는 정보의 자원을 표현해야 한다.
 - 행위에 대한 표현이 아닌 자원을 표현하는데 중점을 두어야 한다.
 - 리소스명은 동사보다는 `명사`로 사용한다.
 ```
 ✅ GET | /user/1 -> user1의 정보를 가져온다.
 ✅ PUT | /user/1 -> user1의 정보를 수정한다.
 ❌ DELETE | /user/delete/1
 ⭕ DELETE | /user/1 -> user1을 삭제한다.
 ```

- 자원에 대한 행위는 `Http Method(GET, POST, PUT, DELETE)`로 표현해야 한다.
```
 ❌ GET | /user/update/1 -> user1의 정보를 입력하는데에 GET 메서드는 맞지 않다.
 ⭕ POST | /user/1 -> user1의 정보를 생성한다.
```

###### 𝙃𝙏𝙏𝙋 𝙈𝙀𝙏𝙃𝙊𝘿
- 𝙂𝙀𝙏, 𝙋𝙊𝙎𝙏, 𝙋𝙐𝙏, 𝘿𝙀𝙇𝙀𝙏𝙀
 - `GET` 리소스를 `조회`하고 해당 도큐먼트에 대한 자세한 정보를 가져온다.
 - `POST` 리소스를 `생성`한다.
 - `PUT` 리소스를 `수정`한다.
 - `DELETE` 리소스를 `삭제`한다.

###### 𝘾𝙤𝙡𝙡𝙚𝙘𝙩𝙞𝙤𝙣, 𝘿𝙤𝙘𝙪𝙢𝙚𝙣𝙩
- 컬렉션과 도큐먼트는 모두 리로스라고 표현할 수 있으며, URI에 표현된다.
- `Document` 하나의 객체
- `Collection` 객체들의 집합
 - 컬렉션을 `복수`로 사용하면 좀 더 직관적이고 이해하기 쉽다.
 ```
 https://www.example.com/foods/korean/stores/1
 -> foods와 stores는 컬렉션, korean,1은 도큐먼트
 ```


# 𝙃𝙏𝙏𝙋 𝙧𝙚𝙨𝙥𝙤𝙣𝙨𝙚 𝙨𝙩𝙖𝙩𝙪𝙨 𝙘𝙤𝙙𝙚𝙨
- `100~199` Informational resposnees
- `200~299` Successful responses
- `300~399` Redirection messages
- `400~499` Client error responses
- `500~599` Server error reesponses

