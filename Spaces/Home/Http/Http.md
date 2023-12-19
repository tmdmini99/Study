# HTTP 프로토콜이란?

HTTP(Hypertext Transfer Protocol)는 인터넷상에서 데이터를 주고 받기 위한 **서버/클라이언트 모델**을 따르는 프로토콜이다.

애플리케이션 레벨의 프로토콜로 **TCP/IP위에서 작동**한다.

HTTP는 **어떤 종류의 데이터든지 전송**할 수 있도록 설계돼 있다. 

HTTP로 보낼 수 있는 데이터는 **HTML문서, 이미지, 동영상, 오디오, 텍스트 문서** 등 여러종류가 있다.

하이퍼텍스트 기반으로(Hypertext) 데이터를 전송하겠다(Transfer) = **링크기반으로 데이터에 접속**하겠다는 의미이다.


**1.2 Connectionless & Stateless**

HTTP는 Connectionless 방식으로 작동한다. 

**서버에 연결하고, 요청해서 응답을 받으면 연결을 끊어버린다.** 

기본적으로는 자원 하나에 대해서 하나의 연결을 만든다. 

이런 작동방식은 각각 아래의 장점과 단점을 가진다





**HTTP**(**H**yper**T**ext **T**ransfer **P**rotocol, [문화어](https://ko.wikipedia.org/wiki/%EB%AC%B8%ED%99%94%EC%96%B4 "문화어"): 초본문전송규약, 하이퍼본문전송규약)는 [W3](https://ko.wikipedia.org/wiki/WWW "WWW") 상에서 정보를 주고받을 수 있는 [프로토콜](https://ko.wikipedia.org/wiki/%ED%86%B5%EC%8B%A0_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C "통신 프로토콜")이다. 주로 [HTML](https://ko.wikipedia.org/wiki/HTML "HTML") 문서를 주고받는 데에 쓰인다. 주로 [TCP](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C "전송 제어 프로토콜")를 사용하고 HTTP/3부터는 [UDP](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9A%A9%EC%9E%90_%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C "사용자 데이터그램 프로토콜")를 사용하며, 80번 포트를 사용한다. [1996년](https://ko.wikipedia.org/wiki/1996%EB%85%84 "1996년") 버전 1.0, 그리고 [1999년](https://ko.wikipedia.org/wiki/1999%EB%85%84 "1999년") 1.1이 각각 발표되었다.

HTTP는 [클라이언트](https://ko.wikipedia.org/wiki/%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8 "클라이언트")와 [서버](https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B2%84 "서버") 사이에 이루어지는 요청/응답(request/response) 프로토콜이다. 예를 들면, 클라이언트인 [웹 브라우저](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80 "웹 브라우저")가 HTTP를 통하여 서버로부터 웹페이지(HTML)나 그림 정보를 요청하면, 서버는 이 요청에 응답하여 필요한 정보를 해당 사용자에게 전달하게 된다. 이 정보가 모니터와 같은 출력 장치를 통해 사용자에게 나타나는 것이다.

HTTP를 통해 전달되는 자료는 http:로 시작하는 [URL](https://ko.wikipedia.org/wiki/URL "URL")(인터넷 주소)로 조회할 수 있다.


## 웹을 만드는 기술들

![](https://velog.velcdn.com/images/hyemin0111/post/40f49153-76fc-4c6c-9ef5-c1ab5c9b3347/image.png)

![](https://velog.velcdn.com/images/hyemin0111/post/ebe3f8ae-010d-4099-9228-f8db483b7a04/image.png)  
위의 3개를 웹 표준이라고 부름. 클라이언트 사이드 스크립트. 클라이언트 컴퓨터에서 동작하는 코드들. -> 클라이언트가 조작 가능  
서버에 저장되고 웹 브라우저에서 받아와서 실행

![](https://velog.velcdn.com/images/hyemin0111/post/79298b33-1872-47e8-baed-17538de029be/image.png)  
서버에 저장된 웹 표준 데이터들을 받아오는 것이 HTTP  
HTTPS -> SSL/TLS  
HTTPS는 HTTP에 보안적인 요소(SSL) 추가한 것

![](https://velog.velcdn.com/images/hyemin0111/post/97d3c267-ce65-466e-a9c8-8772c43e17b1/image.png)  
웹 서버 페이지를 만드는 기술들  
서버 컴퓨터에서 실행되는 코드(↔️HTML,CSS,JS)  
서버에서 실행시키고 결과만 클라이언트에 보내줘서 코드 자체를 볼 수 없음(↔️HTML,CSS,JS: 클라이언트가 해당 코드들을 받아와서 실행)

요새는 Python언어로도 백엔드 개발을 함. 이런 애들을 Restful API라고 한다고 함

## HTTP 프로토콜

### HTTP 프로토콜의 특징

**HyperText Transfer Protocol(하이퍼 텍스트 전송 프로토콜)**  
HyperText

www에서 쓰이는 핵심 프로토콜로 문서의 전송을 위해 쓰이며, 오늘날 거의 모든 웹 애플리케이션에서 사용되고 있다.  
➡️음성, 화상 등 여러 종류의 데이터를 MIME로 정의하여 전송 가능

#### HTTP 특징

`Request` / `Response` (요청/응답) 동작에 기반하여 서비스 제공

#### HTTP 1.0의 특징

"연결 수립, 동작, 연결 해제"의 단순함이 특징  
➡️하나의 URL은 하나의 TCP 연결  
HTML 문서를 전송 받은 뒤 연결을 끊고 다시 연결하여 데이터를 전송한다.

#### HTTP 1.0의 문제점

단순 동작(연결 수립, 동작, 연결 해제)이 반복되어 통신 부하 문제 발생  
ex) 네이버 메인 페이지에 그림이 1개 있으면 연결,동작,해제가 10번 반복됨

![](https://velog.velcdn.com/images/hyemin0111/post/d1f80e43-b93b-4445-9471-e775233a150b/image.png)

#### HTTP 1.1의 특징

HTTP 1.0과 호환 가능  
Multiple Request 처리가 가능하여 Client의 Request가 많을 경우  
연속적인 응답 제공 ➡️ Pipeline 방식의 Request/Response 진행

HTTP 1.0과는 달리 Server가 갖는 하나의 IP Address와 다수의 Web Site 연결 가능

#### HTTP 1.1

빠른 속도와 Internet Protocol 설계에 최적화될 수 있도록 Cache 사용  
Data를 압축해서 전달이 가능하도록 하여 전달하는 Data 양이 감소

![](https://velog.velcdn.com/images/hyemin0111/post/23c07a84-4188-4970-84e4-31484d2921cb/image.png)




---
참조 - https://velog.io/@hyemin0111/HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%EC%9D%B4%EB%9E%80

https://shlee0882.tistory.com/107

