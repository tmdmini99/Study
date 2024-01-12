# intellij에서의 세팅

![[SSL적용1.png]]


tomcat 설정으로 들어가서

![[SSL적용2.png]]

위와 같이 http의 port와 https의 포트를 넣어준다. 자신의 환경에 맞게 넣어주면 된다! 대신 tomcat의 설정도 변경

이러면 intellij의 설정은 끝

3. ssl 인증서를 tomcat에 적용

intellij는 내장 tomcat을 사용하지 않고 실제 tomcat을 자동으로 잡아서 사용하고 있다. 해당 경로를 확인한 후 tomcat의 server.xml 파일을 수정해줘야한다!


![[SSL적용3.png]]

위의 tomcat 설정에서 Startup/Connection 탭을 클릭하면 표시된 위치에 내 tomcat의 경로를 확인할 수 있다. 자신의 tomcat폴더로 이동하자

{tomcat경로}/conf/server.xml

파일을 에디터 파일이나 메모장으로 열어준뒤에


```xml
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
clientAuth="false" sslProtocol="TLS"
keystorePass="패스워드" keystoreFile="파일경로"/>
```

패스워드와 파일경로를 자신의 설정에 맞게 변경해준뒤 적용시켜준다.

아 그리고 port번호의 경우 자신의 설정에 맞게 전부 변경해주면 된다! 이러면 intellij로 http와 https를 모두 요청해도 화면을 정상적으로 받아볼 수 있고 https의 경우 인증서가 인증되지 않은 인증서기때문에


![[SSL적용4.png]]


오류처럼 뜨는데 고급 눌러서 (안전하지 않음)으로 이동 눌러서 사용하면 된다!





---
참조 - https://ililil9482.tistory.com/114