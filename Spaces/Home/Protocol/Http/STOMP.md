## **STOMP**


WebSocket 프로토콜은 두 가지 유형의 메세지를 정의하고 있지만 그 메세지의 내용까지는 정의하고 있지 않는다. 

**STOMP (Simple Text Oriented Messaging Protocol)**은 메세징 전송을 효율적으로 하기 위해 탄생한 프로토콜이고, 기본적으로 pub / sub 구조로 되어있어 메세지를 전송하고 메세지를 받아 처리하는 부분이 확실히 정해져 있기 때문에 개발자 입장에서 명확하게 인지하고 개발할 수 있는 이점이 있다. 한 줄로 정의하자면, **STOMP 프로토콜은 WebSocket 위에서 동작하는 프로토콜로써 클라이언트와 서버가 전송할 메세지의 유형, 형식, 내용들을 정의하는 매커니즘**이다.

또한 STOMP를 이용하면 **메세지의 헤더에 값을 줄 수 있어** 헤더 값을 기반으로 통신 시 인증 처리를 구현하는 것도 가능하며 STOMP 스펙에 정의한 규칙만 잘 지키면 여러 언어 및 플랫폼 간 메세지를 상호 운영할 수 있다.

### **STOMP란**

STOMP는 TCP 또는 WebSocket 같은 양방향 네트워크 프로토콜 기반으로 동작한다.

이름에서 알 수 있듯, STOMP는 Text 지향 프로토콜이나, Message Payload에는 Text or Binary 데이터를 포함 할 수 있다.

위에서 언급한 **pub / sub**란 메세지를 공급하는 주체와 소비하는 주체를 분리해 제공하는 메세징 방법이다. 기본적인 컨셉을 예로 들자면 우체통(Topic)이 있다면 집배원(Publisher)이 신문을 우체통에 배달하는 행위가 있고, 우체통에 신문이 배달되는 것을 기다렸다가 빼서 보는 구독자(Subscriber)의 행위가 있다. 이때 구독자는 다수가 될 수 있다. pub / sub 컨셉을 채팅방에 빗대면 다음과 같다.

> **채팅방 생성** : pub / sub 구현을 위한 Topic이 생성됨  
>   
> **채팅방 입장** : Topic 구독  
>   
> **채팅방에서 메세지를 송수신** : 해당 Topic으로 메세지를 송신(pub), 메세지를 수신(sub)

클라이언트는 메세지를 전송하기 위해 **SEND, SUBSCRIBE COMMAND**를 사용할 수 있다. 또한, **SEND, SUBSCRIBE COMMAND** 요청 Frame에는 메세지가 무엇이고, 누가 받아서 처리할지에 대한 Header 정보가 포함되어 있다.

이런 명령어들은 "destination" 헤더를 요구하는데 이것이 어디에 전송할지, 혹은 어디에서 메세지를 구독할 것 인지를 나타낸다.

위와 같은 과정을 통해 STOMP는 Publish-Subscribe 매커니즘을 제공한다. 즉 Broker를 통해 타 사용자들에게 메세지를 보내거나 서버가 특정 작업을 수행하도록 메세지를 보낼 수 있게 된다.

만약 **Spring에서 지원하는 STOMP를 사용하면 Spring WebSocket 어플리케이션은 STOMP Broker**로 동작하게 된다.

Spring에서 지원하는 STOMP는 많은 기능을 하는데 예를 들어 Simple In-Memory Broker를 이용해 SUBSCRIBE 중인 다른 클라이언트들에게 메세지를 보내준다. Simple In Memory Broker는 클라이언트의 SUBSCRIBE 정보를 자체적으로 메모리에 유지한다.

또한  RabbitMQ, ActiveMQ같은 외부 메세징 시스템을 STOMP Broker로 사용할 수 있도록 지원한다.

구조적인 면을 보자면, **스프링은 메세지를 외부 Broker에게 전달**하고, **Broker는 WebSocket으로 연결된 클라이언트에게 메세지를 전달**하는 구조가 되겠다. 이와 같은 구조 덕분에 HTTP 기반의 보안 설정과 공통된 검증 등을 적용할 수 있게 된다.

**STOMP는 HTTP 에서 모델링되는 Frame 기반 프로토콜**이다. Frame은 몇 개의 Text Line으로 지정된 구조인데 첫 번째 라인은 Text이고 이후 Key:Value 형태로 Header의 정보를 포함한다. 다음 빈 라인을 추가하고 Payload가 존재한다. 이 구조를 보면 HTTP 요청과 왜 유사한지 알 수 있다.

> **COMMAND**  
> header1:value1  
> header2:value2  
>   
> Body^@

- COMMAND : SEND, SUBSCRIBE를 지시할 수 있다.
- header : 기존의 WebSocket으로는 표현이 불가능한 header를 작성할 수 있다.
    - destination : 이 헤더로 메세지를 보내거나(SEND), 구독(SUBSCRIBE)할 수 있다.

**destination**는 의도적으로 정보를 불분명하게 정의하였는데, 이는 STOMP 구현체에서 문자열 구문에 따라 직접 의미를 부여하도록 하기위함이다.

따라서 destination 정보는 STOMP 서버 구현체마다 달라질 수 있기 때문에 각 구현체의 스펙을 살펴보아야 한다.

그러나 일반적으로 다음의 형식을 따른다.

> "topic/.." --> publish-subscribe (1:N)  
> "queue/" --> point-to-point (1:1)

다음은 ClientA가 5번 채팅방에 대해 **구독**을 하는 예시이다.

> **SUBSCRIBE**  
> **destination**: /topic/chat/room/5  
> **id**: sub-1  
>   
> ^@

다음은 ClientB에서 채팅 메세지를 보내는 예시이다.

> **SEND**  
> **destination**: /pub/chat  
> **content-type**: application/json  
>   
> **{"chatRoomId": 5, "type": "MESSAGE", "writer": "clientB"}** ^@

STOMP **서버는 모든 구독자에게 메세지를 Broadcasting하기 위해 MESSAGE COMMAND**를 사용할 수 있다. 서버는 내용을 기반(chatRoomId)으로 메세지를 전송할 broker에 전달한다. (topic을 sub로 보아도 될 것 같다.)

> **MESSAGE**  
> **destination:** /topic/chat/room/5  
> **message-i**d: d4c0d7f6-1  
> **subscription:** sub-1  
>   
> **{"chatRoomId": 5, "type": "MESSAGE", "writer": "clientB"}** ^@