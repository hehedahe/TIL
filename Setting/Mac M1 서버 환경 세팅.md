
1. Homebrew 설치
https://brew.sh

echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/corretto-mm-012/.zprofile

echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/corretto-mm-012/.zprofile

eval "$(/opt/homebrew/bin/brew shellenv)"

Brew -v 으로 버전 확인하기


2. MySQL 설치
https://formulae.brew.sh/formula/mysql

brew install mysql


3. JDK 11 설치
https://formulae.brew.sh/formula/openjdk

brew install openjdk@11

> **M1에 Java17 또는 Open JDK 설치**
	[https://www.azul.com/downloads/#download-openjdk](https://www.azul.com/downloads/#download-openjdk)
	[https://huilife.tistory.com/58](https://huilife.tistory.com/58)


4. Redis 설치
https://formulae.brew.sh/formula/redis

brew install redis


5. workbench 설치(DB tool)
https://dev.mysql.com/downloads/workbench/


6. STS4 설치
https://spring.io/tools


7. VSC 설치
https://code.visualstudio.com/Download#
/Applications/SpringToolSuite4.app/Contents/Eclipse/SpringToolSuite4.ini
vsc로 열기 
-Xverify:none
-XX:+UseParallelGC
-XX:-UseConcMarkSweepGC
-XX:PermSize=32M
-XX:MaxPermSize=128M

- Swagger viewer 플러그인 설치




추가로 해야할 것들

- [ ] MySQL 세부설정
- [ ] JDK환경변수 설정
