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
> 
> 
> ![[STOMP1.jpg]]

**서버는 불분명한 메세지를 전송할 수 없다.** 그러므로 서버의 모든 메세지는 특정 클라이언트 구독에 응답하여야 하고, 서버 메세지의 **"subscription-id" 헤더는 클라이언트 구독의 "id"헤더와 일치**해야 한다.

### **Client Frames**

#### **SEND**

SEND frame은 destination의 메세징 시스템으로 메세지를 보낸다. 필수 헤더는 어디로 보낼지에 대한 "destination" 하나이다. SEND frame의 body는 보내고자 하는 메세지이다.

> **SEND**  
> **destination**: /queue/a  
> **content-type**: text/plain  
>   
> hello queue a  
> ^@

SEND Frame은 body가 있는 경우 "content-length"와 "content-type"헤더를 반드시 가져야만 한다..

#### **SUBSCRIBE**

SUBSCRIBE frame은 주어진 destination에 등록하기 위해 사용된다. SEND frame과 마찬가지로 Subscribe는 client가 구독하기 원하는 목적지를 가리키는 "destination" 헤더를 필요로 한다. 가입된 대상에서 수신된 모든 메세지는 이후 MESSAGE frame로서 서버에서 클라이언트에게 전달된다.

> **SUBSCRIBE**  
> **id**: 0  
> **destination**: /queue/foo  
> **ack**: client  
>   
> ^@

단일 연결은 여러 개의 구독을 할 수 있으므로 구독 ID를 고유하게 식별하기 위해 "id"헤더가 프레임에 포함되어야 한다.

이외에도 아래 내용이 있지만 아직 용도도 모르기에 추후 해보면서 필요성을 느끼면 정리하도록 한다.

- UNSUBSCRIBE
- BEGIN
- COMMIT
- ABORT
- ACK
- NACK
- DISCCONECT

![[STOMP2.png]]


### **STOMP Benefits**

Spring framework 및 Spring Security는 STOMP 를 사용하여 WebSocket만 사용할 때보다 더 다채로운 모델링을 할 수 있다.

- Messaging Protocol을 만들고 메세지 형식을 커스터마이징 할 필요가 없다.
- RabbitMQ, ActiveMQ 같은 Message Broker를 이용해, Subscription(구독)을 관리하고 메세지를 브로드캐스팅할 수 있다.
- WebSocket 기반으로 각 Connection(연결)마다 WebSocketHandler를 구현하는 것 보다 @Controller 된 객체를 이용해 조직적으로 관리할 수 있다.
    - 즉, 메세지는 STOMP의 "destination" 헤더를 기반으로 @Controller 객체의 @MethodMapping 메서드로 라우팅 된다.
- STOMP의 "destination" 및 Message Type을 기반으로 메세지를 보호하기 위해 Spring Security를 사용할 수 있다.

### **Enable STOMP**

Spring은 WebSocket / SockJS를 기반으로 STOMP를 위해 spring-messaging과 spring-websocket 모듈을 제공한다.

아래 예시와 같이, STOMP 설정을 할 수 있는데 기본적으로 커넥션을 위한 STOMP Endpoint를 설정해야만 한다.

```
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;

@Configuration
@EnableWebSocketMessageBroker
public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
    
        registry.addEndpoint("/example").withSockJS();  
    }

    @Override
    public void configureMessageBroker(MessageBrokerRegistry config) {
    
        config.setApplicationDestinationPrefixes("/test"); 
        config.enableSimpleBroker("/topic", "/queue"); 
    }
}
```

- /example는 WebSocket 또는 SockJS Client가 웹소켓 핸드셰이크 커넥션을 생성할 경로이다.
- /test 경로로 시작하는 STOMP 메세지의 "destination" 헤더는 @Controller 객체의 @MessageMapping 메서드로 라우팅된다.
- 내장된 메세지 브로커를 사용해 Client에게 Subscriptions, Broadcasting 기능을 제공한다. 또한 /topic, /queue로 시작하는 "destination" 헤더를 가진 메세지를 브로커로 라우팅한다.

내장된 **Simple Message Broker**는 **/topic, /queue prefix**에 대해 특별한 의미를 부여하지 않는다.

### **Using STOMP**

SockJS로 브라우저에 연결하기 위해 sockjs-client를 이용할 수 있다. STOMP에 있어 많은 어플리케이션들은 jmesnil/stomp-websocket( stomp.js 로 알려진 )라이브러리를 사용해왔지만, 더이상 유지되지 않는다. 최근에는 JSteunou/webstomp-client를 많이 사용한다.

```
var sock = new SockJS("/ws/chat");
var stomp = webstomp.over(sock);

stomp.connect({}, function(frame) {
}

/* WebSocket만 이용할 경우 */

var websocket = new WebSocket("/ws/chat");
var stomp = webstomp.over(websocket);

stomp.connect({}, function(frame) {
}
```

More Example Codes...

- [Using WebSocket to build an interactive web application - a getting started guide](https://spring.io/guides/gs/messaging-stomp-websocket/)
- [Stock Portfolio - a sample application](https://github.com/rstoyanchev/spring-websocket-portfolio)

### **Flow of Messages**

STOMP Endpoint가 노출되고 나면, Spring 어플리케이션은 연결되어있는 Client들에 대해 STOMP 브로커가 된다

(/app == /pub, /topic == /sub) 아래 그림은 내장 메세지 브로커를 사용한 경우 컴포넌트 구성을 보여준다.

![[STOMP3.png]]


**spring**-**message** 모듈은 Spring framework의 통합된 Messaging 어플리케이션을 위한 지원을 한다.

- **Message** : headers와 payload를 포함하는 메세지의 표현
- **MessageHandler** : Message 처리에 대한 계약
- **SimpleAnnotationMethod** : @MessageMapping 등 Client의 SEND를 받아서 처리한다.
- **SimpleBroker** : Client의 정보를 메모리 상에 들고 있으며, Client로 메세지를 보낸다.
- channel
    - **clientInboundChannel** : WebSocket Client로부터 들어오는 요청을 전달하며, WebSocketMessageBrokerConfigurer를 통해 intercept, taskExecutor를 설정할 수 있다.
        - 클라이언트로 받은 메세지를 전달
    - **clientOutboundChannel** : WebSocket Client로 Server의 메세지를 내보내며, WebSocketMessageBrokerConfigurer를 통해 intercept, taskExecutor를 설정할 수 있다.
        - 클라이언트에게 메세지를 전달
    - **brokerChannel** : Server 내부에서 사용하는 채널이며, 이를 통해 SimpleAnnotationMethod는 SimpleBroker의 존재를 직접 알지 못해도 메세지를 전달할 수 있다.
        - 서버의 어플리케이션 코드 내에서 브로커에게 메세지를 전달

---

이것들 말고도 더 많은 내용이 있지만, 지금 다 보기에는 힘들 것 같다. 추후 Spring docs를 더 참고하도록 하고, 실제 코드를 짜서 구현해보도록 하자.

### **STOMP 적용**

#### **Dependency Injection**

stomp-websocket DI를 해주자. 아래는 Gradle 환경에서 사용되었다.

> // https://mvnrepository.com/artifact/org.webjars/stomp-websocket  
> implementation group: 'org.webjars', name: 'stomp-websocket', version: '2.3.3-1'

#### WebSocketConfig **-> StompWebSocketConfig**

기존에 WebSocketConfigurer를 구현한 WebSocketConfig 설정파일을 WebSocketMessageBrokerConfigurer를 구현한 StompWebSocketConfig로 변경한다.

```
import org.springframework.context.annotation.Configuration;
import org.springframework.messaging.simp.config.MessageBrokerRegistry;
import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

@EnableWebSocketMessageBroker
@Configuration
public class StompWebSocketConfig implements WebSocketMessageBrokerConfigurer {

    //endpoint를 /stomp로 하고, allowedOrigins를 "*"로 하면 페이지에서
    //Get /info 404 Error가 발생한다. 그래서 아래와 같이 2개의 계층으로 분리하고
    //origins를 개발 도메인으로 변경하니 잘 동작하였다.
    //이유는 왜 그런지 아직 찾지 못함
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {
        registry.addEndpoint("/stomp/chat")
                .setAllowedOrigins("http://localhost:8080")
                .withSockJS();
    }

    /*어플리케이션 내부에서 사용할 path를 지정할 수 있음*/
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {
        registry.setApplicationDestinationPrefixes("/pub");
        registry.enableSimpleBroker("/sub");
    }
}
```

- **@EnableWebSocketMessageBroker**
    - Stomp를 사용하기위해 선언하는 어노테이션
- **setApplicationDestinationPrefixes** : Client에서 SEND 요청을 처리  
    - Spring docs에서는 /topic, /queue로 나오나 편의상 /pub, /sub로 변경
- **enableSimpleBroker**
    - 해당 경로로 SimpleBroker를 등록. SimpleBroker는 해당하는 경로를 SUBSCRIBE하는 Client에게 메세지를 전달하는 간단한 작업을 수행
- **enableStompBrokerRelay** 
    - SimpleBroker의 기능과 외부 Message Broker( RabbitMQ, ActiveMQ 등 )에 메세지를 전달하는 기능을 가짐

#### **ChatMessageDTO**

```
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class ChatMessageDTO {

    private String roomId;
    private String writer;
    private String message;
}
```

#### **ChatRoomDTO**

더이상 하나의 채팅방에 옹기종기 모이는 것이 아닌 private한 공간에서 채팅할 수 있도록 채팅방을 만들어주자.

```
import lombok.Getter;
import lombok.Setter;
import org.springframework.web.socket.WebSocketSession;

import java.util.HashSet;
import java.util.Set;
import java.util.UUID;

@Getter
@Setter
public class ChatRoomDTO {

    private String roomId;
    private String name;
    private Set<WebSocketSession> sessions = new HashSet<>();
    //WebSocketSession은 Spring에서 Websocket Connection이 맺어진 세션

    public static ChatRoomDTO create(String name){
        ChatRoomDTO room = new ChatRoomDTO();

        room.roomId = UUID.randomUUID().toString();
        room.name = name;
        return room;
    }
}
```

#### **ChatRoomRepository**

채팅방을 생성하고 정보를 조회하는 Repository를 생성한다. 지금은 간단히 구현해보고자 하는 것이 목표이므로 채팅방 정보를 Map Collection을 사용하여 관리하나, 실제 서비스 시 DB를 사용해 저장해야한다.

```
import org.gorany.community.dto.ChatRoomDTO;
import org.springframework.stereotype.Repository;

import javax.annotation.PostConstruct;
import java.util.*;
import java.util.stream.Stream;

@Repository
public class ChatRoomRepository {

    private Map<String, ChatRoomDTO> chatRoomDTOMap;

    @PostConstruct
    private void init(){
        chatRoomDTOMap = new LinkedHashMap<>();
    }

    public List<ChatRoomDTO> findAllRooms(){
        //채팅방 생성 순서 최근 순으로 반환
        List<ChatRoomDTO> result = new ArrayList<>(chatRoomDTOMap.values());
        Collections.reverse(result);

        return result;
    }

    public ChatRoomDTO findRoomById(String id){
        return chatRoomDTOMap.get(id);
    }

    public ChatRoomDTO createChatRoomDTO(String name){
        ChatRoomDTO room = ChatRoomDTO.create(name);
        chatRoomDTOMap.put(room.getRoomId(), room);

        return room;
    }
}
```

#### **StompChatController**

```
import lombok.RequiredArgsConstructor;
import org.gorany.community.dto.ChatMessageDTO;
import org.springframework.messaging.handler.annotation.MessageMapping;
import org.springframework.messaging.simp.SimpMessagingTemplate;
import org.springframework.stereotype.Controller;

@Controller
@RequiredArgsConstructor
public class StompChatController {

    private final SimpMessagingTemplate template; //특정 Broker로 메세지를 전달

    //Client가 SEND할 수 있는 경로
    //stompConfig에서 설정한 applicationDestinationPrefixes와 @MessageMapping 경로가 병합됨
    //"/pub/chat/enter"
    @MessageMapping(value = "/chat/enter")
    public void enter(ChatMessageDTO message){
        message.setMessage(message.getWriter() + "님이 채팅방에 참여하였습니다.");
        template.convertAndSend("/sub/chat/room/" + message.getRoomId(), message);
    }

    @MessageMapping(value = "/chat/message")
    public void message(ChatMessageDTO message){
        template.convertAndSend("/sub/chat/room/" + message.getRoomId(), message);
    }
}
```

@**MessageMapping** 을 통해 WebSocket으로 들어오는 메세지 발행을 처리한다. Client에서는 prefix를 붙여 "/pub/chat/enter"로 발행 요청을 하면 Controller가 해당 메세지를 받아 처리하는데, 메세지가 발행되면 "/sub/chat/room/[roomId]"로 메세지가 전송되는 것을 볼 수 있다.

Client에서는 해당 주소를 **SUBSCRIBE**하고 있다가 메세지가 전달되면 화면에 출력한다. 이때 /sub/chat/room/[roomId]는 채팅방을 구분하는 값이다.

기존의 핸들러 ChatHandler의 역할을 대신 해주므로 핸들러는 없어도 된다.

#### **RoomController**

채팅화면을 보여주기 위한 Controller.

```
import lombok.RequiredArgsConstructor;
import lombok.extern.log4j.Log4j2;
import org.gorany.community.dto.ChatRoomDTO;
import org.gorany.community.repository.ChatRoomRepository;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

import java.util.List;

@Controller
@RequiredArgsConstructor
@RequestMapping(value = "/chat")
@Log4j2
public class RoomController {

    private final ChatRoomRepository repository;

    //채팅방 목록 조회
    @GetMapping(value = "/rooms")
    public ModelAndView rooms(){

        log.info("# All Chat Rooms");
        ModelAndView mv = new ModelAndView("chat/rooms");

        mv.addObject("list", repository.findAllRooms());

        return mv;
    }

    //채팅방 개설
    @PostMapping(value = "/room")
    public String create(@RequestParam String name, RedirectAttributes rttr){

        log.info("# Create Chat Room , name: " + name);
        rttr.addFlashAttribute("roomName", repository.createChatRoomDTO(name));
        return "redirect:/chat/rooms";
    }

    //채팅방 조회
    @GetMapping("/room")
    public void getRoom(String roomId, Model model){

        log.info("# get Chat Room, roomID : " + roomId);

        model.addAttribute("room", repository.findRoomById(roomId));
    }
}
```

#### **View**

**rooms.html (채팅방 목록)**

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns:sec="http://www.thymeleaf.org/extras/spring-security">

<th:block th:replace="~{/layout/basic :: setContent(~{this :: content})}">
    <th:block th:fragment="content">
        <div class="container">
            <div>
                <ul th:each="room : ${list}">
                    <li><a th:href="@{/chat/room(roomId=${room.roomId})}">[[${room.name}]]</a></li>
                </ul>
            </div>
        </div>
        <form th:action="@{/chat/room}" method="post">
            <input type="text" name="name" class="form-control">
            <button class="btn btn-secondary">개설하기</button>
        </form>
    </th:block>
</th:block>

</html>
```

```
        <script th:inline="javascript">
            $(document).ready(function(){

                var roomName = [[${roomName}]];

                if(roomName != null)
                    alert(roomName + "방이 개설되었습니다.");

                $(".btn-create").on("click", function (e){
                    e.preventDefault();

                    var name = $("input[name='name']").val();

                    if(name == "")
                        alert("Please write the name.")
                    else
                        $("form").submit();
                });

            });
        </script>
```

**room.html (채팅방 상세)**

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org" xmlns:sec="http://www.thymeleaf.org/extras/spring-security">

<th:block th:replace="~{/layout/basic :: setContent(~{this :: content})}">
    <th:block th:fragment="content">

        <div class="container">
            <div class="col-6">
                <h1>[[${room.name}]]</h1>
            </div>
            <div>
                <div id="msgArea" class="col"></div>
                <div class="col-6">
                    <div class="input-group mb-3">
                        <input type="text" id="msg" class="form-control">
                        <div class="input-group-append">
                            <button class="btn btn-outline-secondary" type="button" id="button-send">전송</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-6"></div>
        </div>


        <script src="https://cdn.jsdelivr.net/npm/sockjs-client@1/dist/sockjs.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/stomp.js/2.3.3/stomp.min.js"></script>
        
    </th:block>
</th:block>

</html>
```

```
        <script th:inline="javascript">
            $(document).ready(function(){

                var roomName = [[${room.name}]];
                var roomId = [[${room.roomId}]];
                var username = [[${#authentication.principal.username}]];

                console.log(roomName + ", " + roomId + ", " + username);

                var sockJs = new SockJS("/stomp/chat");
                //1. SockJS를 내부에 들고있는 stomp를 내어줌
                var stomp = Stomp.over(sockJs);

                //2. connection이 맺어지면 실행
                stomp.connect({}, function (){
                   console.log("STOMP Connection")

                   //4. subscribe(path, callback)으로 메세지를 받을 수 있음
                   stomp.subscribe("/sub/chat/room/" + roomId, function (chat) {
                       var content = JSON.parse(chat.body);

                       var writer = content.writer;
                       var str = '';

                       if(writer === username){
                           str = "<div class='col-6'>";
                           str += "<div class='alert alert-secondary'>";
                           str += "<b>" + writer + " : " + message + "</b>";
                           str += "</div></div>";
                           $("#msgArea").append(str);
                       }
                       else{
                           str = "<div class='col-6'>";
                           str += "<div class='alert alert-warning'>";
                           str += "<b>" + writer + " : " + message + "</b>";
                           str += "</div></div>";
                           $("#msgArea").append(str);
                       }

                       $("#msgArea").append(str);
                   });

                   //3. send(path, header, message)로 메세지를 보낼 수 있음
                   stomp.send('/pub/chat/enter', {}, JSON.stringify({roomId: roomId, writer: username}))
                });

                $("#button-send").on("click", function(e){
                    var msg = document.getElementById("msg");

                    console.log(username + ":" + msg.value);
                    stomp.send('/pub/chat/message', {}, JSON.stringify({roomId: roomId, message: msg.value, writer: username}));
                    msg.value = '';
                });
            });
        </script>
```

(주석 출처 및 코드 참고: [supawer0728.github.io/2018/03/30/spring-websocket/](https://supawer0728.github.io/2018/03/30/spring-websocket/))

#### **TEST 결과**

위의 코드로 **Chrome**, **Firefox**, **Edge**, **Mobile** **Chrome**에서 테스트 해보았다. 결과는 만족하리만큼 잘 동작하였다. **Internet** **Explorer**에서도 테스트해보았으나, **IE** 11, 10, 9까지는 탈 없이 잘 동작한다. 하지만 IE 8의 경우 JQuery가 적용되지 않아 이에 대한 문제가 있어 동작하지 않을 뿐 통신에는 문제없다.

현재 **WebSocket** -> **SockJS** -> **STOMP** 순으로 진행해보았는데, 정리를 하고 가자면

**WebSocket**을 사용하면 Streaming, Polling 보다 실시간에 가깝게 처리할 수 있고 트래픽이 줄어든다.

**SockJS**를 사용하면 WebSocket을 지원하지 않는 브라우저에서도 WebSocket Emulation을 이용해 웹소켓을 이용하는 것 처럼 동작하게 해준다.

**STOMP**를 사용하면 Session을 직접 관리하지 않아도 되고, 메세지의 처리 방식이 간편해진다.



![[STOMP4.png]]


![[STOMP5.png]]


![[STOMP6.png]]




