
## **WebSocket이란** 

Transport protocol의 일종으로 쉽게 이야기하면 웹버전의 TCP 또는 Socket이라고 이해하면 된다.

WebSocket은 서버와 클라이언트 간에 Socket Connection을 유지해서 언제든 양방향 통신 또는 데이터 전송이 가능하도록 하는 기술이다.

Real-time web application구현을 위해 널리 사용되어지고 있다. (SNS어플리케이션, LoL같은 멀티플레이어 게임, 구글 Doc, 증권거래, 화상채팅 등)

# TCP/IP소켓과 웹소켓의 차이

(TCP/IP) 소켓에 대한 내용은 [여기](https://velog.io/@rhdmstj17/%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC-1)서 볼 수 있다. 이제 소켓은 대충 알겠고 .. 웹소켓은 뭘까?

일단 소켓과 웹소켓은 **IP와 포트를 통한 통신**을 한다는 점에서는 **비슷하다.** 또, 둘 다 양방향 통신을 한다는 비슷한 특징을 갖고 있기도 하다.

차이점은,  **웹 소켓은 HTTP 레이어에서 작동하는 소켓으로 TCP/IP 소켓의 레이어와 다름**  


## HTTP의 한계

- 비연결성 (단방향)
- 매번 연결 **맺고 끊는** 과정의 비용
- **request-response** 의 구조
- 헤더의 비중이 너무 큼 → 실시간성으로 많은 데이터를 주고 받고자 하는 경우에는 매우 부담이 되는 요소 중 하나
[[http]]는 위와 같은 특징을 같고 있기 때문이다. 즉, 요청을 보내면 응답이 오는 **단방향적 구조로 통신**하기 때문에 TCP/IP 프로토콜을 사용하는 소켓처럼 계속 connection이 유지되는 **실시간 통신을 할 수 없다.** 이의 문제점을 해결하기 위해 나온것이 웹소켓 프로토콜이다.

## 웹소켓 특징

- 연결지향 (양방향)
	- 데이터 송수신을 `동시에` 처리할 수 있는 통신 방법
	- 클라이언트와 서버가 서로에게 원할 때, 데이터를 주고 받을 수 있다.
	- 통상적인 HTTP 통신은 Client가 요청을 보내는 경우에만 Server가 응답하는 `단방향 통신`
- 한 번 **연결 맺은 뒤 유지**
- 핸드쉐이크 과정에서는 헤더의 비중이 크지만, 한번 연결이 되면 간단한 메시지들만 오고감 → 굉장히 경제적임
1. 실시간 네트워킹(Real Time-Networking)
    - 웹 환경에서 연속된 데이터를 빠르게 노출
        - ex) 채팅, 주식, 비디오 데이터, …
    - 여러 단말기에 빠르게 데이터를 교환

웹소켓은 http 레이어 위에서 작동한다.

## 웹소켓이 쓰이는 곳


**case1** | B유저와 게임을 할때 타유저가 게임 action을 줄 때마다 새로고침을 해야함 → 매우 불편하겠죠? 즉, 게임할 때  
**case 2** | 채팅을 해야할 때  
**case 3** | 실시간 주식 거래 사이트를 만들 때 .. 등등

## 웹 소켓 동작 방법

### 1. 일단 손을 맞잡는다 🤝 핸드쉐이킹!

채널에 대한 정상적인 통신이 시작되기 전에 두 개의 실체 간에 확립된 통신 채널의 변수를 동적으로 설정하는 자동화된 협상과정


![[WebSocket1.png]]

### 2. 이제 데이터를 넣을  Frame 구성

최초 접속에서만 http 프로토콜 위에서 핸드쉐이킹을 하기 때문에 http 헤더를 사용한다.  
웹소켓을 위한 별도의 포트는 없으며 기존 포트(http-80,https-443)를 사용(호환된다는 의미)  
프레임으로 구성된 메시지라는 논리적 단위로 송수신  
메시지에 포함될 수 있는 교환 가능한 메시지(frame)는 텍스트와 바이너리만 가능



![[WebSocket2.png]]

웹 소켓 동작 과정은 크게 세가지로 나눌 수 있다.

위 이미지의 빨간 색 박스에 해당하는 **Opening Handshake**  
위 이미지의 노란 색 박스에 해당하는 **Data transfer**  
위 이미지의 보라 색 박스에 해당하는 **Closing Handshake**


(1) [Opening Handshake](https://tools.ietf.org/html/rfc6455#section-4)

웹소켓 클라이언트에서 핸드쉐이크 요청(HTTP Upgrade)을 전송하고 이에 대한 응답으로 핸드 셰이크 응답을 받는데, 이때 응답 코드는 101입니다. 101은 '프로토콜 전환'을 서버가 승인했음을 알리는 코드입니다.


```bash
// 연결 수립을 요청할 때의 HTTP 헤더
GET /chat HTTP/1.1
Host: server.com 
Upgrade: websocket 
Connection: Upgrade 
Sec-WebSocket-Key: dGgnnsduwqka17gdsSB3j==  
Origin: http://example.com  
Sec-WebSocket-Protocol: chat, superchat 
Sec-WebSocket-Version: 13 
```

- GET /chat HTTP/1.1
    - 연결 수립 과정은 HTTP 프로토콜 사용
    - HTTP 버전은 1.1 이상 사용
    - 반드시 GET 메서드를 사용
- Host
    - 웹소켓 서버의 주소
- Upgrade
	- 프로토콜을 전환하기 위해 사용하는 헤더
    - 현재 클라이언트, 서버, 전송 프로토콜 연결에서 다른 프로토콜로 업그레이드 또는 변경하기 위한 규칙
- Connection
	- 현재의 전송이 완료된 후 네트워크 접속을 유지할 것인가에 대한 정보
	- Upgrade 헤더 필드가 명시되었을 경우, 송신자는 반드시 Upgrade 옵션을 지정한 Connection 헤더 필드로 전송
- Sec-WebSocket-Key
	- 유효한 요청인지 확인하기 위해 사용하는 키 값. 
    - 길이가 16바이트인 임의로 선택된 숫자를 base64로 인코딩 한 값
    - 클라이언트와 서버 간 서로의 신원을 인증하기 위한 키
- Origin
    - 클라이언트로 웹 브라우저를 사용하는 경우 필수항목, 클라이언트의 주소
- Sec-WebSocket-Protocol
    - 클라이언트가 요청하는 여러 `서브프로토콜`을 의미
    - 공백문자로 구분하며 순서에 따라 우선권이 부여
    - 서버에서 여러 프로토콜 혹은 프로토콜 버전을 나눠서 서비스할 경우 필요한 정보
    - 사용하고자 하는 하나 이상의 웹 소켓 프로토콜 지정. 필요한 경우에만 사용

```bash
// HTTP 응답 헤더
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRb+jfgh=
```

- 101 Switching Protocols
    - 101 Switching Protocols가 Response로 오면 웹소켓이 연결됐다는 의미
- Sec-WebSocket-Accept
    - 클라이언트로 받은 Sec-WebSocket-Key를 사용하여 계산된 값
    - 클라이언트에서 계산한 값과 일치하지 않으면 연결 수립이 되지 않음.




(2) [Data Transfer](https://tools.ietf.org/html/rfc6455#section-5)

핸드쉐이크를 통해 웹소켓 연결이 수립되면, 데이터 전송 파트가 시작됩니다. 여기에서는 클라이언트와 서버가 '메시지'라는 개념으로 데이터를 주고받는데, 여기서 메시지는 한 개 이상의 '프레임'으로 구성되어 있습니다. (프레임은 텍스트(UTF-8) 데이터, 바이너리 데이터, 컨트롤 프레임(프로토콜 레벨의 신호) 등이 있습니다)

핸드 셰이크가 끝난 시점부터 서버와 클라이언트는 서로가 살아 있는지 확인하기 위해 heartbeat 패킷을 보내며, 주기적으로 ping을 보내 체크합니다. 이는 서버와 클라이언트 양측에서 설정 가능합니다. 

(3) [Close Handshake](https://tools.ietf.org/html/rfc6455#section-7)

클라이언트와 서버 모두 커넥션을 종료하기 위한 컨트롤 프레임을 전송할 수 있습니다. 이 컨트롤 프레임은 Closing Handshake를 시작하라는 특정한 컨트롤 시퀀스를 포함한 데이터를 가지고 있습니다. 위 그림에서는 서버가 커넥션을 종료한다는 프레임을 보내고, 클라이언트가 이에 대한 응답으로 Close 프레임을 전송합니다. 그러면 웹소켓 연결이 종료됩니다. 연결 종료 이후에 수신되는 모든 추가적인 데이터는 버려집니다





## 웹 소켓 프로토콜 특징

- 최초 접속시에만 http프로토콜 위에서 handshaking을 하기 때문에 http header를 사용한다
- 웹소켓을 위한 별도의 포트는 없고, 기존 포트를 사용한다
- 프레임으로 구성된 메시지라는 논리적 단위로 송수신 한다
- 메시지에 포함될 수 있는 교환 가능한 메시지는 텍스트와 바이너리 뿐이다




## 웹소켓 한계

- 웹소켓은 HTML5 이후로 등장한 기술이므로, HTML5 이전의 기술로 구현된 서비스에서 사용할 수 없음.

### 1. Socket.io, SockJS

- HTML5 이전의 기술로 구현된 서비스에서 웹 소컷처럼 사용 할 수 있도록 도와주는 기술
- JavaScript를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현
- 브라우저와 웹 서버의 종류와 버전을 파악하여 가장 적합한 기술을 선택하여 사용하는 방식

### 2. STOMP

```null
웹소켓은 문자열들을 주고받을 수 있게 해줄 뿐 그 이상의 일은 하지 않는다. 주고 받는 문자열의 해독은 온전히 어플리케이션에 맡긴다

HTTP는 형식을 정해두었기 때문에 모두가 약속을 따르기만 하면 해석할 수 있다. 하지만 WebSocker방식은 sub-protocols을 사용해서 주고 받는 메시지의 형태를 약속하는 경우가 많음

sub-protocol로 자주 쓰이는게 STOMP

- STOMP
```





### 웹소켓(WebSockets) 사용하기

웹소켓은 클라이언트와 서버 간의 실시간 양방향 통신을 가능하게 해줍니다. Spring과 Node.js 모두 웹소켓을 지원하므로, 이를 사용하여 데이터베이스 업데이트 알림을 전송할 수 있습니다.

#### 1.1 Spring Boot에서 웹소켓 설정하기

Spring Boot에서 웹소켓을 설정하는 방법은 다음과 같습니다.

**의존성 추가 (pom.xml)**



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>


<dependency>  
    <groupId>org.springframework</groupId>  
    <artifactId>spring-web</artifactId>  
    <version>${org.springframework-version}</version>  
</dependency>
```


WebSocketConfig.java

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {
    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
        config.enableSimpleBroker("/topic"); // 클라이언트가 구독할 주제
        config.setApplicationDestinationPrefixes("/app");
    }

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/ws").withSockJS(); // 클라이언트에서 연결할 엔드포인트
    }
}

```


controller
```java
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UpdateController {
    private final SimpMessagingTemplate messagingTemplate;

    public UpdateController(SimpMessagingTemplate messagingTemplate) {
        this.messagingTemplate = messagingTemplate;
    }

    @PostMapping("/update")
    public void updateData() {
        // 데이터베이스 업데이트 로직
        // ...

        // 클라이언트에 알림 전송
        messagingTemplate.convertAndSend("/topic/updates", "데이터가 업데이트되었습니다.");
    }
}

```

#### 1.2 Node.js에서 웹소켓 클라이언트 설정하기

Node.js에서 웹소켓 클라이언트를 설정하려면 `socket.io-client` 패키지를 사용할 수 있습니다.

**설치**



```bash
npm install socket.io-client
```

Node.js 코드

```js
const io = require('socket.io-client');
const socket = io('http://your-spring-server-address/ws');

socket.on('connect', () => {
    console.log('Connected to Spring WebSocket server');
});

socket.on('/topic/updates', (message) => {
    alert(message); // 클라이언트에서 alert 띄우기
});
```



### 2. 폴링(Polling) 기법 사용하기

폴링은 일정 시간 간격으로 서버에 요청을 보내어 상태를 확인하는 방법입니다. 이 방법은 실시간성이 떨어지지만, 간단하게 구현할 수 있습니다.



2.1 Spring Boot에서 데이터 상태 확인 API 만들기


```java
@RestController
public class StatusController {
    private boolean dataUpdated = false;

    @PostMapping("/update")
    public void updateData() {
        // 데이터베이스 업데이트 로직
        // ...

        // 업데이트 상태 변경
        dataUpdated = true;
    }

    @GetMapping("/check-update")
    public boolean checkUpdate() {
        boolean updated = dataUpdated;
        dataUpdated = false; // 한 번 확인한 후 초기화
        return updated;
    }
}

```



2.2 Node.js에서 폴링 설정하기


```js
setInterval(() => {
    fetch('http://your-spring-server-address/check-update')
        .then(response => response.json())
        .then(data => {
            if (data) {
                alert('데이터가 업데이트되었습니다.');
            }
        });
}, 5000); // 5초마다 체크
```



### 결론

- **웹소켓**을 사용하면 실시간으로 데이터베이스 업데이트를 감지할 수 있어 사용자 경험을 개선할 수 있습니다.
- **폴링** 기법은 구현이 간단하지만, 서버에 더 많은 요청을 보내게 되어 부하가 증가할 수 있습니다.




app.post



### 1. Spring Boot에서 POST 요청 보내기

Spring Boot에서 데이터베이스 업데이트가 이루어질 때, Node.js 서버에 HTTP POST 요청을 보내는 코드를 추가합니다.

#### 1.1 Spring Boot에서 HttpClient 설정하기

먼저, Spring Boot에서 HTTP 클라이언트를 사용하기 위해 `spring-boot-starter-web` 의존성이 필요합니다.

**의존성 추가 (pom.xml)**



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```


#### 1.2 데이터베이스 업데이트 시 POST 요청 보내기

**UpdateController.java**


```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
public class UpdateController {
    
    private final RestTemplate restTemplate;

    public UpdateController(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    @PostMapping("/update")
    public ResponseEntity<String> updateData() {
        // 데이터베이스 업데이트 로직
        // ...

        // Node.js 서버에 POST 요청 보내기
        String nodeUrl = "http://your-node-server-address/notify-update";
        restTemplate.postForEntity(nodeUrl, null, String.class);

        return ResponseEntity.ok("Data updated successfully");
    }
}

```


### 2. Node.js 서버에서 POST 요청 수신하기

Node.js 서버에서 Spring Boot로부터 POST 요청을 수신하고 알림을 띄우는 코드를 작성합니다.

#### 2.1 Express 설정하기

Express 서버를 설정하고 POST 요청을 처리하는 라우트를 만듭니다.

**Node.js 코드**


```js
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const port = 3000;

app.use(bodyParser.json()); // JSON 요청 본문을 파싱하기 위해 middleware 추가

app.post('/notify-update', (req, res) => {
    // 요청 수신 시 alert를 띄울 수 없으므로, 콘솔에 로그를 남김
    console.log('데이터가 업데이트되었습니다.'); // 알림
    // 클라이언트에 알림을 띄우려면, 웹소켓이나 다른 방법을 사용해야 함

    // 응답 반환
    res.send('Notification received');
});

app.listen(port, () => {
    console.log(`Node.js server listening at http://localhost:${port}`);
});
```


### 3. 클라이언트에서 알림 띄우기

Node.js 서버에서 직접 클라이언트에게 알림을 띄우는 것은 불가능합니다. 일반적으로는 웹소켓이나 AJAX 요청을 통해 클라이언트와의 통신을 구현합니다. 하지만, 단순한 알림을 위해 Node.js 서버에서 클라이언트에게 추가적인 요청을 보내도록 설정할 수 있습니다.

#### 3.1 AJAX를 사용한 알림 구현

클라이언트에서 정기적으로 Node.js 서버에 요청을 보내어 데이터 업데이트 상태를 확인하도록 설정할 수 있습니다. 아래는 예시 코드입니다.

**클라이언트 코드 (예: HTML)**


```html
<script>
setInterval(() => {
    fetch('http://your-node-server-address/check-update') // Node.js 서버의 알림 상태 확인 API
        .then(response => response.json())
        .then(data => {
            if (data.updated) {
                alert('데이터가 업데이트되었습니다.');
            }
        });
}, 5000); // 5초마다 확인
</script>
```



- **Spring Boot**에서 데이터베이스 업데이트가 발생하면 **Node.js** 서버에 HTTP POST 요청을 보내어 업데이트를 알립니다.
- **Node.js**는 요청을 수신하고, 클라이언트에서 알림을 띄우기 위해 별도의 요청을 처리하는 로직을 추가할 수 있습니다.
- 클라이언트 측에서 정기적으로 Node.js 서버에 요청을 보내어 업데이트 상태를 확인하는 방법도 가능합니다.


### 웹소켓 (WebSocket)

#### 장점

1. **실시간 통신**: 웹소켓은 양방향 통신을 지원하므로 서버와 클라이언트 간에 실시간으로 데이터를 주고받을 수 있습니다. 예를 들어, 데이터가 업데이트될 때마다 클라이언트에 즉시 알림을 보낼 수 있습니다.
    
2. **효율성**: 웹소켓은 HTTP 핸드셰이크 후, 지속적인 연결을 유지하여 데이터 전송 시 오버헤드가 적습니다. 즉, 연결이 계속 유지되므로 매번 요청을 보내는 것보다 효율적입니다.
    
3. **이벤트 기반**: 서버가 클라이언트의 상태를 감지하고 이벤트를 발생시킬 수 있으므로 사용자 경험이 더 원활합니다.
    

#### 단점

1. **복잡성**: 웹소켓을 구현하는 데 필요한 코드가 더 복잡합니다. 서버와 클라이언트 모두에 대해 웹소켓 연결과 이벤트 처리를 구현해야 합니다.
    
2. **서버 리소스 사용**: 지속적인 연결을 유지해야 하므로 서버의 리소스 사용량이 늘어날 수 있습니다. 특히 클라이언트 수가 많아질 경우, 성능에 영향을 줄 수 있습니다.
    
3. **브라우저 호환성**: 대부분의 최신 브라우저에서 지원하지만, 특정 환경에서는 문제가 발생할 수 있습니다.
    

### HTTP POST

#### 장점

1. **간단한 구현**: HTTP POST 방식은 RESTful API를 사용하는 것과 유사하게 구현할 수 있으며, 구조가 간단합니다. Spring Boot와 Express.js와 같은 프레임워크에서 쉽게 사용할 수 있습니다.
    
2. **상태 비저장**: 요청과 응답이 독립적이므로, 서버가 클라이언트의 상태를 관리할 필요가 없습니다.
    
3. **보안**: HTTPS를 사용하여 데이터를 안전하게 전송할 수 있으며, CSRF(사이트 간 요청 위조)와 같은 보안 문제를 더 쉽게 관리할 수 있습니다.
    

#### 단점

1. **비실시간 통신**: 클라이언트가 서버에 요청을 보내야 하므로 실시간성이 떨어집니다. 데이터 업데이트 시마다 클라이언트에서 주기적으로 요청을 보내야 합니다.
    
2. **오버헤드**: 매번 HTTP 요청을 보내고 응답을 받는 과정에서 오버헤드가 발생합니다. 특히 빈번한 요청을 보내야 하는 경우 성능 저하가 발생할 수 있습니다.


websocket-context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:websocket="http://www.springframework.org/schema/websocket"
       xmlns:messaging="http://www.springframework.org/schema/messaging"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/websocket http://www.springframework.org/schema/websocket/spring-websocket.xsd
                           http://www.springframework.org/schema/messaging http://www.springframework.org/schema/messaging/spring-messaging.xsd">

    <!-- WebSocket 설정 활성화 -->
    <websocket:annotation-driven />

    <!-- WebSocket STOMP 엔드포인트 설정 -->
    <bean id="stompEndpointRegistry" class="org.springframework.web.socket.config.annotation.StompEndpointRegistry">
        <property name="addEndpoint" value="/messages" />
        <property name="withSockJS" value="true" />
    </bean>

    <!-- 메시지 브로커 설정 -->
    <bean id="messageBroker" class="org.springframework.messaging.simp.config.MessageBrokerRegistry">
        <property name="enableSimpleBroker" value="/queue,/topic" />
        <property name="setApplicationDestinationPrefixes" value="/app" />
    </bean>

</beans>

```

pom.xml
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-websocket</artifactId>
    <version>5.3.0</version> <!-- 버전은 사용 중인 Spring 버전에 맞게 설정 -->
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-messaging</artifactId>
    <version>5.3.0</version> <!-- 버전은 사용 중인 Spring 버전에 맞게 설정 -->
</dependency>
```




`TextWebSocketHandler`와 `WebSocketHandler`는 Spring WebSocket에서 사용되는 두 가지 주요 WebSocket 핸들러 인터페이스입니다. 각각의 차이점과 용도에 따라 어떤 것이 더 좋은지 결정할 수 있습니다.

### 1. **`WebSocketHandler`**

`WebSocketHandler`는 WebSocket 연결을 처리하는 기본 인터페이스입니다. 이 인터페이스는 WebSocket 세션과 메시지를 처리하는 메서드를 정의합니다. 이 메서드들을 구현하여 WebSocket의 연결, 메시지 전송, 오류 처리 등을 직접 구현할 수 있습니다.

**주요 메서드**:

- `afterConnectionEstablished(WebSocketSession session)` : WebSocket 연결이 설정되었을 때 호출됩니다.
- `handleMessage(WebSocketSession session, WebSocketMessage<?> message)` : 클라이언트에서 전송한 메시지를 처리합니다.
- `handleTransportError(WebSocketSession session, Throwable exception)` : 전송 중 오류가 발생하면 호출됩니다.
- `afterConnectionClosed(WebSocketSession session, CloseStatus closeStatus)` : 연결이 종료된 후 호출됩니다.
- `supportsPartialMessages()` : 부분 메시지를 지원할지 여부를 결정합니다.

**장점**:

- 매우 유연하고 기본적인 WebSocket 처리 방식입니다. 여러 종류의 메시지 형식을 직접 처리하고 싶을 때 유용합니다.
- 기본적인 WebSocket 핸들러로, 추가적인 기능을 원하는 경우 자신만의 로직을 작성할 수 있습니다.

### 2. **`TextWebSocketHandler`**

`TextWebSocketHandler`는 `WebSocketHandler`를 구현한 추상 클래스입니다. 이 클래스는 텍스트 기반 메시지(WebSocket에서 가장 흔히 사용되는 메시지 포맷)를 처리할 때 유용합니다. `WebSocketHandler`의 메서드를 기본적으로 구현하며, 텍스트 메시지 처리만 집중할 수 있도록 설계되어 있습니다.

`TextWebSocketHandler`를 사용하면, `handleTextMessage` 메서드만 구현하면 됩니다. 이를 통해 텍스트 메시지 처리에 집중할 수 있습니다.

**주요 메서드**:

- `handleTextMessage(WebSocketSession session, TextMessage message)` : 텍스트 메시지를 처리하는 메서드입니다.
- `afterConnectionEstablished(WebSocketSession session)` : 연결이 설정되었을 때 호출됩니다.
- `afterConnectionClosed(WebSocketSession session, CloseStatus closeStatus)` : 연결이 종료된 후 호출됩니다.
- `handleTransportError(WebSocketSession session, Throwable exception)` : 오류가 발생했을 때 호출됩니다.

**장점**:

- 텍스트 메시지 처리에 특화되어 있어, 기본적인 메시지 처리 로직이 이미 구현되어 있어 더욱 간단하게 사용할 수 있습니다.
- 텍스트 메시지만 처리할 경우, 코드가 간결해지고 구현이 쉬워집니다.

### **어떤 것을 선택해야 할까?**

- **텍스트 기반의 메시지 처리만 필요하다면**: `TextWebSocketHandler`가 더 적합합니다. 기본적인 텍스트 메시지 처리만 필요하다면, `handleTextMessage`만 구현하면 되므로 코드가 더 간결하고 관리하기 쉽습니다.
    
- **다양한 메시지 형식(텍스트 외에도 바이너리, 커스텀 메시지 등)을 처리해야 한다면**: `WebSocketHandler`를 사용하는 것이 더 좋습니다. `WebSocketHandler`는 메시지 형식에 구애받지 않으며, 다양한 종류의 메시지를 처리할 수 있도록 유연하게 설계되었습니다. `handleMessage` 메서드를 오버라이드하여 텍스트뿐만 아니라 바이너리 데이터나 다른 형태의 메시지도 처리할 수 있습니다.
    

### **결론**

- **`TextWebSocketHandler`**: 텍스트 메시지 처리만 필요한 경우 사용.
- **`WebSocketHandler`**: 다양한 메시지 형식이나 복잡한 로직이 필요한 경우 사용.



```java
package com.kpop.merch.common.filter;  
  
import com.kpop.merch.common.handler.SessionTimeoutWebSocketHandler;  
import lombok.var;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Component;  
import org.springframework.web.context.support.WebApplicationContextUtils;  
  
import javax.servlet.ServletContext;  
import javax.servlet.annotation.WebListener;  
import javax.servlet.http.HttpSessionEvent;  
import javax.servlet.http.HttpSessionListener;  
  
@Component  
public class SessionTimeoutListener implements HttpSessionListener {  
  
  
    @Autowired  
    private SessionTimeoutWebSocketHandler webSocketHandler;  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        // 세션 생성 시 로그 출력  
        System.out.println("세션 생성: " + se.getSession().getId());  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        // 세션 만료 시 로그 출력  
        System.out.println("세션 만료: " + se.getSession().getId());  
        if (webSocketHandler == null) {  
            ServletContext servletContext = se.getSession().getServletContext();  
            var ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);  
            webSocketHandler = ctx.getBean(SessionTimeoutWebSocketHandler.class);  
        }  
        // 세션 만료 시 WebSocketHandler의 notifySessionExpired 호출  
            if (webSocketHandler != null) {  
                webSocketHandler.notifySessionExpired();  
            } else {  
                System.out.println("WebSocketHandler가 초기화되지 않았습니다.");  
            }  
  
  
    }  
}
```



component로 bean id 지정

```java
package com.kpop.merch.common.handler;  
  
  
import org.springframework.stereotype.Component;  
import org.springframework.web.socket.CloseStatus;  
import org.springframework.web.socket.TextMessage;  
import org.springframework.web.socket.WebSocketSession;  
import org.springframework.web.socket.handler.TextWebSocketHandler;  
  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
import java.util.concurrent.CopyOnWriteArraySet;  
  
@Component("webSocketHandler")  
public class SessionTimeoutWebSocketHandler  extends TextWebSocketHandler{  
  
    private static final CopyOnWriteArraySet<WebSocketSession> sessions = new CopyOnWriteArraySet<>();  
  
    @Override  
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {  
        super.afterConnectionEstablished(session);  
  
        // HTTP 세션을 WebSocket 세션에 연결하기 (Optional: HTTP 세션에 있는 사용자 정보를 사용하려는 경우)  
        HttpSession httpSession = (HttpSession) session.getAttributes().get("HTTP_SESSION");  
  
        if (httpSession != null) {  
            // HTTP 세션에서 사용자 ID를 가져와 WebSocket 세션에 사용자 정보를 추가  
            String userId = (String) httpSession.getAttribute("userId");  
            if (userId != null) {  
                session.getAttributes().put("userId", userId);  // WebSocket 세션에 사용자 ID 추가  
                System.out.println("2");  
            }  
        } else {  
            // HTTP 세션이 없다면 기본 사용자 정보로 설정  
            session.getAttributes().put("user", "user123");  // 예시로 사용자 ID 추가  
            System.out.println("1");  
        }  
  
        // 세션을 리스트에 추가  
        sessions.add(session);  
  
        // 디버깅용 로그  
        System.out.println("WebSocket connected: " + session.getId());  
        System.out.println("Session Attributes: " + session.getAttributes());  
        System.out.println("세션 추가됨: " + session.getId());  
        System.out.println("Total connected sessions: " + sessions.size());  // 현재 연결된 세션 수 확인  
    }  
  
  
    @Override  
    public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {  
        super.afterConnectionClosed(session, status);  
        // 세션 종료 시 리스트에서 해당 세션 제거  
        notifySessionExpired();  // 세션 만료 알림을 먼저 보냄  
        sessions.remove(session);  
        System.out.println("WebSocket disconnected: " + session.getId());  
        System.out.println("세션 종료됨: " + session.getId());  
        System.out.println("Total connected sessions: " + sessions.size());  // 현재 연결된 세션 수 확인  
  
        // 이미 종료된 세션에서는 메시지를 보내지 않음  
        if (!session.isOpen()) {  
            System.out.println("세션이 이미 닫혀 있습니다. 메시지를 보내지 않습니다.");  
        }  
    }  
  
    public void notifySessionExpired() {  
        System.out.println("세션 만료 알림 발송");  
  
        // 세션이 닫힌 상태인지 확인  
        for (WebSocketSession session : sessions) {  
            System.out.println("세션 상태 확인: " + session.getId() + " 상태: " + (session.isOpen() ? "열려 있음" : "닫힘"));  
            if (session.isOpen()) {  
                try {  
                    System.out.println("들어오나?");  
                    TextMessage message = new TextMessage("Session expired. Please log in again.");  
                    session.sendMessage(message);  
                } catch (IOException e) {  
                    e.printStackTrace();  
                }  
            } else {  
                System.out.println("세션이 이미 닫혔습니다. 메시지를 보내지 않습니다.");  
            }  
            System.out.println("들어오나?2");  
        }  
        System.out.println("들어오나3?");  
    }  
  
}
```


`nullPointerException`이 발생하는 이유가 `@Component`와 `@Autowired`를 사용한 의존성 주입이 제대로 작동하지 않는 경우라면, 이는 보통 다음과 같은 원인일 수 있습니다:

1. **`SessionTimeoutListener`가 스프링 컨텍스트 외부에서 생성되었을 가능성**이 큽니다. `@WebListener`와 같이 서블릿 컨테이너에서 직접 관리하는 리스너는 스프링이 빈으로 관리하지 않으므로, `@Autowired`를 통한 주입이 이루어지지 않습니다.
2. **의존성 주입이 제대로 이루어지지 않아 빈이 `null` 상태**로 남게 됩니다.

이 경우 `SessionTimeoutListener`가 스프링 컨텍스트에서 관리될 수 있도록 설정을 수정하거나, 수동으로 스프링 컨텍스트에서 `webSocketHandler` 빈을 가져오는 방식으로 해결할 수 있습니다.

다음은 이러한 문제를 해결하는 두 가지 방법입니다.

---

### 방법 1: `SessionTimeoutListener`를 스프링에서 관리하도록 변경

`SessionTimeoutListener`를 빈으로 등록하고, 세션 리스너로 사용할 때 스프링 컨텍스트에서 생성된 빈을 등록하도록 `WebApplicationInitializer`나 XML 설정을 통해 지정할 수 있습니다.

#### 1. `SessionTimeoutListener`에 `@Component` 사용

현재 `@Component`가 선언되어 있다면, 그대로 유지합니다.

```java
@Component
public class SessionTimeoutListener implements HttpSessionListener {
    @Autowired
    private SessionTimeoutWebSocketHandler webSocketHandler;

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        webSocketHandler.notifySessionExpired();
    }
}

```

#### 2. `WebApplicationInitializer`에서 세션 리스너 설정

스프링 부트가 아닌 레거시 스프링에서 서블릿을 설정하는 방식으로 `WebApplicationInitializer`를 사용할 수 있습니다. 아래와 같이 세션 리스너를 등록하고, 스프링 빈을 컨텍스트에서 가져오도록 합니다.

```java
import org.springframework.web.WebApplicationInitializer;
import org.springframework.web.context.support.WebApplicationContextUtils;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;

public class AppInitializer implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        // 스프링 애플리케이션 컨텍스트에서 SessionTimeoutListener 빈을 가져와 등록
        var ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);
        var listener = ctx.getBean(SessionTimeoutListener.class);
        servletContext.addListener(listener);
    }
}

```

### 방법 2: 스프링 컨텍스트에서 직접 빈 가져오기

세션 리스너 내부에서 `webSocketHandler`를 직접 스프링 컨텍스트에서 가져오는 방법입니다.

#### `SessionTimeoutListener` 코드 수정

```java
import org.springframework.web.context.support.WebApplicationContextUtils;
import javax.servlet.ServletContext;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class SessionTimeoutListener implements HttpSessionListener {

    private SessionTimeoutWebSocketHandler webSocketHandler;

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        if (webSocketHandler == null) {
            ServletContext servletContext = se.getSession().getServletContext();
            var ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);
            webSocketHandler = ctx.getBean(SessionTimeoutWebSocketHandler.class);
        }
        webSocketHandler.notifySessionExpired();
    }
}
```


`ws://localhost:8080/ws/notify` 맞춰줘야함

```js
const socket = new WebSocket("ws://localhost:8080/ws/notify");  
socket.onopen = () => {  
  console.log("WebSocket connected");  
};  
socket.onclose = () => {  
  console.log("WebSocket connection closed");  
};  
socket.onmessage = function(event) {  
    console.log("Received message: ", event.data);  // 받은 메시지 출력  
    if (event.data === "Session expired. Please log in again.") {  
        alert("세션이 만료되었습니다. 다시 로그인해 주세요.");  
        window.location.href = "/login/login";  
    }  
};
```






web.xml
```xml
<listener>  
    <listener-class>com.kpop.merch.common.filter.SessionTimeoutListener</listener-class>  
</listener>
```


servlet-context.xml
```xml
<websocket:handlers>  
    <websocket:mapping path="/ws/notify" handler="webSocketHandler"/>  
</websocket:handlers>
```








---
참조 -  https://velog.io/@rhdmstj17/%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC-2  - 실시간 통신

 웹소켓- https://gusrb3164.github.io/web/2021/10/28/websocket-socket/
https://urmaru.com/7
https://velog.io/@stbpiza/%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C


https://velog.io/@bnb8419/Socket.io%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%8B%A4%EC%8B%9C%EA%B0%84-%EC%B1%84%ED%8C%85%EA%B5%AC%ED%98%84 


https://velog.io/@codingbotpark/Web-Socket-%EC%9D%B4%EB%9E%80


https://kellis.tistory.com/65


https://velog.io/@ctdlog/Web-Socket


https://velog.io/@suhyun_zip/%EC%9B%B9%EC%86%8C%EC%BC%93-%EC%B1%84%ED%8C%85-%EA%B8%B0%EB%8A%A5-23.09.04