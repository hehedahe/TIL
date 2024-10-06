옵시디언으로 생성한 md파일에 이미지를 함께 올리는 경우, 깃에서는 이미지가 보이지 않고 문자열로 나온다.
또한, 캡쳐한 이미지들을 폴더에 한 곳에 잘 저장을 한다고 하더라도 결국 이미지 파일들 용량만큼 깃에 올라가게 되는데 이를 해결해 주기 위한 커뮤니티 플러그인 `imgur`을 사용해 보려고 한다.


플러그인 URL
[obsidian://show-plugin?id=obsidian-imgur-plugin](obsidian://show-plugin?id=obsidian-imgur-plugin)

###### 플러그인 다운로드 받는 방법
1. 설정 - `community plugins` - `browse` 진입
2. imgur 검색하여 다운로드 받는다.
3. imgur 사이트에서 회원가입을 한다.
   SNS로그인도 가능하다. 일반 회원가입을 진행하려고 하면 이용이 불가능한 지역이라고 나오는데 왜 그러는거야 ,,,

여기까지 했으면 imgur 플러그인 설정에서 위에서 가입한 계정을 연동해야 imgur 서버에 업로드할 수 있다.
그럼 옵시디언에서 imgur 서버에 업로드된 이미지 url(아래처럼!)을 사용하여 쉽게 이미지를 관리할 수 있다.
https://i.imgur.com/파일이름

###### imgur 계정 연동하는 방법 
1. 설정 - `imgur` - Images upload approach 를 `Authenticated Imgur upload` 로 변경한다.
![](https://i.imgur.com/dlVFMSn.png)
2. Client ID를 발급받기 위해  [https://api.imgur.com/oauth2/addclient](https://api.imgur.com/oauth2/addclient) 으로 이동한다.
3. 아래 사진처럼 양식을 작성 후 `submit`한다.
	1. Application name: 가입한 username
	2. Authorization type: 디폴트로 선택되어 있음
	3. Authorization callback URL: `obsidian://imgur-oauth`
	4. Email: 가입한 이메일 계정 
![](https://i.imgur.com/36Z9dLX.png)

4. 그러면 아래 사진처럼 Client ID가 발급된다. 
![](https://i.imgur.com/m8nyxif.png)

5. 다시 옵시디언으로 넘어와서 발급받은 Client ID를 넣어주고 `Authenticate` 버튼을 눌러주면 인증창으로 넘어간다. `allow` 버튼을 누르면 끝!
![](https://i.imgur.com/UHoi0px.png)

