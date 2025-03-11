




## Netty란?




> 네티는 비동기 이벤트 기반 네트워크 애플리케이션 프레임워크로써 유지보수를 고려한 고성능 프로토콜 서버와 클라이언트를 개발할 수 있다.  

네티는 비동기 이벤트 기반 네트워크 프레임워크이다. 그럼 비동기는 무엇일까?



## Netty의 주요 특징


동기와 비동기는 여러 의미로 사용되는데, 함수 호출 방식에서의 동기와 비동기에 대해서 알아보자.

### a. 동기 / 비동기

#### 동기(Synchronous)란?

함수 A가 함수 B를 호출하면, A는 다른 작업을 하지 않고 B의 결과(리턴값)만을 기다린다.

즉, 함수 A는 함수 B를 호출하고, 함수 B가 작업이 끝나면 다음 작업을 진행할 수 있다.


![[Netty1.png]]


#### 비동기(Asynchronous )란?

함수 A가 함수 B를 호출할 때, B의 작업 완료 여부를 따지지 않고, A는 **다른 작업을 그대로 진행**한다.

B가 작업이 끝나면, 콜백함수나, 이벤트를 통해 A에게 작업 결과를 전달하는 방식이다.





![[Netty2.png]]


* 비동기 호출을 지원하는 옵저버 패턴, 콜백함수, 리액터 패턴 등 다양하다.

비동기는 I/O 작업과 같은 상대적으로 느린 작업을 수행할 때 I/O 작업을 기다리지 않고, 다른 작업을 그대로 진행할 수 있어 동시에 여러 작업을 진행할 수 있다.

예를 들어, 하나의 요청(주문)에 여러 건의 트랜잭션(User 업데이트, 주문처리, 상품재고 처리 등)으로 묶여 있어 DB 쿼리, 로직을 수행하는 데 많은 시간이 걸릴 것이다. 이럴 경우 동기로 처리하면 대규모 트래픽이 발생할 경우 성능이 저하될 수 있다. 비동기 방식으로 처리하면, 응답을 기다리지 않고 웹 애플리케이션은 다른 요청을 처리할 수 있다. 비동기 방식으로 처리하면, 순간적으로 급증하는 트래픽을 처리하기에 매우 적합할 수 있다.

동기와 비동기는 a가 b를 호출할 때 a가 b의 작업 종료 여부에 관심이 있냐, 없느냐의 차이다.


### b. 블로킹과 논블로킹

블로킹과 논블로킹을 소켓 I/O 관점에서 살펴보자.

소켓의 기본적인 연결과정이다.


![[Netty3.png]]


#### 블로킹(Blocking)

블로킹 모드의 소켓은 서버/클라이언트 I/O 작업에서 해당 작업이 완료되기 전까지 스레드의 블로킹이 발생한다. 스레드는 작업이 완료되기 전까지 중단된 채 대기만 하게 된다.


![[Netty4.png]]



블로킹 소켓은 스레드의 블로킹이 발생하기 때문에  여러 클라이언트의 요청에 대한 처리를 할 수 없다.

그래서 나온 방법이 각각의 클라이언트에게 스레드를 할당하는 모델이다.




![[Netty5.png]]



accept 메서드가 병목지점이다. accept 메서드는 블로킹 방식으로 동작하여 단위 시간에 하나의 연결만을 처리한다. 즉, 여러 클라이언트가 동시에 접속하는 경우 대기시간이 오래 걸린다.

또한 스레드 생성에 제한이 없기 때문에 메모리 부족으로 인해 OOM 오류가 발생할 수 있다.

메모리 부족 문제는 스레드 풀링으로 해결할 수 있다. 하지만, 접속 가능한 클라이언트 수를 스레드 풀 크기에 의존해야 하며 스레드 풀의 수를 최대로 잡아도, GC 과정에서 수행시간이 오래 걸리며 각 스레드는 CPU 점유를 위해 경쟁(컨텍스트 스위칭)하는 데 이때 CPU 자원을 소모하기 때문에 비효율적이다.

블로킹의 핵심은 스레드가 블로킹되어 다음 작업을 진행할 수 없다!

여기까지만 봐도 블로킹 소켓은 동시접속자를 수용하기에는 적합하지 않다. 블로킹 모드 소켓 단점을 개선한 것이 논블로킹 소켓 방식이다. 

#### 논블로킹(Non Blocking)

논블로킹은 즉시 처리할 수 없는 경우라면, 오류를 즉시 리턴되어 호출한 쪽에서 블로킹이 되지 않게 하는 방법이다.

일반적으로 콜이 성공적으로 실행될 때까지 계속 루프를 돌면서 할인하는 방법을 사용한다.

스레드가 블록되지 않기 때문에 멀티스레드를 사용하지 않고도 여러 개의 I/O를 처리할 수 있다.

하지만, 오류 코드를 확인하고 처리하는 부분으로 인해 프로그램 구조가 복잡해질 수 있다.

언뜻 보면 비동기와 논블로킹이 비슷하지만,

비동기는 I/O 처리를 바로 할 수 없을 때 처리가 완료되는 시점까지 백그라운드에서 대기하고, 종료 타이밍을 회신하고,

논블로킹은 I/O 처리가 완료되지 않으면 에러를 회신하여 블록 상태를 만들지 않는다.

**블로킹과 논블로킹의 차이는 블로킹은 스레드에서 블로킹되어 여러 개의 I/O를 처리할 수 없고,  논블로킹은 여러개의 단일 스레드에서 여러개의 I/O를 처리할 수 있다!** 

#### **동기/비동기     vs       블로킹/논블로킹**

동기, 비동기는 호출되는 함수의 작업 완료 여부를 신경쓰느냐가 관심사이고, 블로킹, 논블로킹은 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사이다.

### c. 이벤트 기반 네트워크 프레임워크

각 이벤트를 정의해 놓고, 이벤트가 발생했을 때 실행되는 코드를 준비해 둔다.

논블로킹 소켓의 Selector를 사용한 I/O 이벤트 감시 역시 이벤트 기반의 프로그램이다.

이벤트 기반 프로그래밍은 추상화 수준이 중요하다. 여기서 추상화란 얼마나 작은 단위로 이벤트를 나눌 것인지 이다.

추상화 수준이 고수준이면 세부적으로 제어가 필요하기 때문에 복잡해지고, 너무 저수준이면 한 동작에 많은 이벤트가 발생하여 성능에 좋지 않을 수 있다. 

네트워크 프로그램에서 이벤트는 크게 **소켓 연결**, **데이터 송수신**으로 나눌 수 있으며, Netty에는 다양한 이벤트를 제공한다.

**이벤트 기반 네트워크 구조**

소켓의 데이터를 읽고 쓸 때, 소켓채널(NIO) 또는 스트림에 기록한다. 

Netty는 데이터를 채널에 직접 쓰고 읽지 않으며, 이벤트 핸들러를 통한다.


![[Netty6.png]]



## 4. 정리

- 비동기 호출, 논블로킹 소켓을 구현하기에는 코드가 매우 복잡해진다. Netty는 기능 구현에 집중할 수 있도록 프레임워크 레벨에서 API로 제공한다.
- 이벤트 기반의 네트워크 프레임워크로 이벤트에 따라 로직을 분리할 수 있고, 소켓연결,  데이터 송수신, 예외, 연결 종료 등 다양한 이벤트를 제공한다.


---


### **IntelliJ에서 Netty 프로젝트 설정하는 방법 (Gradle 기반)**

Netty를 IntelliJ IDEA에서 실행하려면 몇 가지 설정이 필요합니다. 아래 절차를 따라 하면 Netty 기반의 간단한 서버를 실행할 수 있습니다.

---

## **1. IntelliJ에서 Netty 프로젝트 생성하기**

### **📌 1) IntelliJ에서 새 프로젝트 만들기**

1. IntelliJ IDEA 실행
2. **"New Project"** 선택
3. **"Gradle"** 선택 (Maven도 가능하지만 Gradle이 관리가 편리함)
4. **"Java"** 선택
5. 프로젝트 이름 설정 (예: `netty-chat-server`)
6. **JDK 버전 선택** (Java 11 이상 추천)
7. **Finish** 클릭

---

## **2. Netty 라이브러리 추가**

### **📌 1) `build.gradle` 수정**

Netty는 Gradle이나 Maven을 사용해 쉽게 추가할 수 있습니다.

#### ✅ **Gradle 프로젝트 (`build.gradle`)**

아래와 같이 **Netty 라이브러리를 추가**합니다.


```gradle
plugins {
    id 'java'
}

group = 'com.example'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'io.netty:netty-all:4.1.100.Final'
    implementation 'org.slf4j:slf4j-api:1.7.36'
    implementation 'org.slf4j:slf4j-simple:1.7.36'
}
```


**📌 추가 후 IntelliJ에서 `Refresh Gradle` 클릭하여 라이브러리를 다운로드하세요.**

---

## **3. Netty 서버 코드 작성**

이제 Netty 기반의 간단한 서버를 만들어 보겠습니다.

### ✅ **📌 Netty 서버 코드 (`NettyServer.java`)**


```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.*;

public class NettyServer {
    private final int port;

    public NettyServer(int port) {
        this.port = port;
    }

    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new HttpServerCodec());  // HTTP 요청을 처리할 수 있도록 설정
                        ch.pipeline().addLast(new HttpObjectAggregator(65536));
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<FullHttpRequest>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, FullHttpRequest request) {
                                System.out.println("Received request: " + request.uri());
                                FullHttpResponse response = new DefaultFullHttpResponse(
                                        request.protocolVersion(),
                                        HttpResponseStatus.OK);
                                ctx.writeAndFlush(response);
                            }
                        });
                    }
                });

            ChannelFuture f = b.bind(port).sync();
            System.out.println("Netty Server started on port: " + port);
            f.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        new NettyServer(8080).run();
    }
}
```


## **4. Netty 서버 실행하기**

### **📌 1) IntelliJ에서 실행**

1. `NettyServer.java` 파일을 열고 `main` 함수 실행
2. 콘솔에 `Netty Server started on port: 8080` 출력되면 성공

### **📌 2) 브라우저에서 확인**

1. 브라우저에서 `http://localhost:8080` 접속
2. 서버에서 요청을 받고 로그 출력됨:


```yaml
Received request: /
```


## **5. Netty 서버 종료**

서버를 종료하려면 IntelliJ 콘솔에서 `Ctrl + C` 또는 `Terminate` 버튼을 클릭하세요.

---

## **6. 추가 기능 (WebSocket 채팅 서버)**

Netty를 이용하여 **WebSocket 채팅 서버**도 만들 수 있습니다.

### ✅ **📌 WebSocket 핸들러 추가**


```java
import io.netty.channel.*;
import io.netty.handler.codec.http.websocketx.*;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.HttpResponseStatus;
import io.netty.handler.codec.http.FullHttpResponse;
import io.netty.handler.codec.http.DefaultFullHttpResponse;
import io.netty.handler.codec.http.FullHttpRequest;
import io.netty.handler.stream.ChunkedWriteHandler;

public class WebSocketServerHandler extends SimpleChannelInboundHandler<Object> {
    private WebSocketServerHandshaker handshaker;

    @Override
    protected void channelRead0(ChannelHandlerContext ctx, Object msg) throws Exception {
        if (msg instanceof FullHttpRequest) {
            FullHttpRequest req = (FullHttpRequest) msg;
            WebSocketServerHandshakerFactory wsFactory =
                    new WebSocketServerHandshakerFactory("ws://localhost:8080/ws", null, false);
            handshaker = wsFactory.newHandshaker(req);
            if (handshaker == null) {
                WebSocketServerHandshakerFactory.sendUnsupportedVersionResponse(ctx.channel());
            } else {
                handshaker.handshake(ctx.channel(), req);
            }
        } else if (msg instanceof WebSocketFrame) {
            WebSocketFrame frame = (WebSocketFrame) msg;
            if (frame instanceof TextWebSocketFrame) {
                String request = ((TextWebSocketFrame) frame).text();
                System.out.println("Received: " + request);
                ctx.channel().writeAndFlush(new TextWebSocketFrame("Server received: " + request));
            }
        }
    }
}
```


위 핸들러를 Netty 서버에 추가하면 **WebSocket 통신**을 사용할 수 있습니다.

---

## **7. 결론**

이제 IntelliJ에서 Netty 프로젝트를 설정하고 실행하는 방법을 배웠습니다! 🚀

✅ **Netty 서버 구축 과정 요약**

1. IntelliJ에서 **Gradle 기반 프로젝트 생성**
2. `build.gradle`에 **Netty 라이브러리 추가**
3. **Netty 서버 코드 작성**
4. **실행 후 브라우저에서 테스트 (`http://localhost:8080`)**
5. **WebSocket 기반 채팅 서버도 쉽게 구현 가능!**



### **Maven 프로젝트에 Netty 추가 및 구현 방법**

✅ **이미 Maven 프로젝트를 만들었다면, Netty를 추가하여 바로 구현할 수 있습니다!**  
아래 절차를 따르면 Maven 기반 프로젝트에서도 Netty를 쉽게 설정할 수 있습니다.

---

## **1. Maven 프로젝트에 Netty 추가하기**

### **📌 `pom.xml`에 Netty 라이브러리 추가**

Netty를 사용하려면 **Maven 의존성(dependency)**을 추가해야 합니다.


```xml
<dependencies>
    <!-- Netty Core -->
    <dependency>
        <groupId>io.netty</groupId>
        <artifactId>netty-all</artifactId>
        <version>4.1.100.Final</version>
    </dependency>

    <!-- SLF4J (로그) -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.36</version>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.36</version>
    </dependency>
</dependencies>
```


📌 **의존성 추가 후** IntelliJ에서 `Maven -> Reload Project` 버튼을 클릭하여 업데이트하세요.

---

## **2. Netty 서버 구현**

### ✅ **Netty 기본 서버 (`NettyServer.java`)**

이제 Netty 서버 코드를 추가해서 실행할 수 있습니다.


```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.*;

public class NettyServer {
    private final int port;

    public NettyServer(int port) {
        this.port = port;
    }

    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new HttpServerCodec());
                        ch.pipeline().addLast(new HttpObjectAggregator(65536));
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<FullHttpRequest>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, FullHttpRequest request) {
                                System.out.println("Received request: " + request.uri());
                                FullHttpResponse response = new DefaultFullHttpResponse(
                                        request.protocolVersion(),
                                        HttpResponseStatus.OK);
                                ctx.writeAndFlush(response);
                            }
                        });
                    }
                });

            ChannelFuture f = b.bind(port).sync();
            System.out.println("Netty Server started on port: " + port);
            f.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        new NettyServer(8080).run();
    }
}
```


## **3. Netty 서버 실행하기**

### ✅ **IntelliJ에서 실행**

1. `NettyServer.java` 파일에서 `main` 함수 실행
2. 콘솔에 아래 메시지가 나오면 성공


```yaml
Netty Server started on port: 8080
```


1. **브라우저에서 `http://localhost:8080`로 접속**하면 서버가 요청을 받을 수 있습니다.

---

## **4. WebSocket 서버 추가 (선택)**

Netty를 활용하여 **WebSocket 기반 채팅 서버**도 추가할 수 있습니다.

### ✅ **📌 WebSocket 핸들러 (`WebSocketServerHandler.java`)**


```java
import io.netty.channel.*;
import io.netty.handler.codec.http.websocketx.*;
import io.netty.handler.codec.http.*;

public class WebSocketServerHandler extends SimpleChannelInboundHandler<Object> {
    private WebSocketServerHandshaker handshaker;

    @Override
    protected void channelRead0(ChannelHandlerContext ctx, Object msg) throws Exception {
        if (msg instanceof FullHttpRequest) {
            FullHttpRequest req = (FullHttpRequest) msg;
            WebSocketServerHandshakerFactory wsFactory =
                    new WebSocketServerHandshakerFactory("ws://localhost:8080/ws", null, false);
            handshaker = wsFactory.newHandshaker(req);
            if (handshaker == null) {
                WebSocketServerHandshakerFactory.sendUnsupportedVersionResponse(ctx.channel());
            } else {
                handshaker.handshake(ctx.channel(), req);
            }
        } else if (msg instanceof WebSocketFrame) {
            WebSocketFrame frame = (WebSocketFrame) msg;
            if (frame instanceof TextWebSocketFrame) {
                String request = ((TextWebSocketFrame) frame).text();
                System.out.println("Received: " + request);
                ctx.channel().writeAndFlush(new TextWebSocketFrame("Server received: " + request));
            }
        }
    }
}
```


📌 **위 핸들러를 Netty 서버에 추가하면 WebSocket 채팅 서버로 확장할 수 있습니다.**

---

## **5. 결론**

✅ **Maven 프로젝트에서도 Netty를 쉽게 추가하여 구현 가능**  
✅ `pom.xml`에 Netty 의존성 추가 후, Netty 서버 코드를 실행하면 됨  
✅ WebSocket을 추가하면 **실시간 채팅 서버도 가능**

💡 **이미 만든 Maven 프로젝트에서 Netty를 추가로 구현할 수 있습니다!**



## **1. Spring Boot에서 Netty를 활용하는 방법**

Spring Boot에서 Netty를 사용할 수 있는 방법은 크게 **3가지**가 있습니다.

### ✅ **(1) Spring Boot + WebFlux (Netty를 내장 서버로 사용)**

- Spring Boot에서 기본 웹 서버(Tomcat)를 **Netty로 변경**하여 사용할 수 있음.
- **Spring WebFlux**는 Netty 기반으로 동작하는 비동기 웹 프레임워크.

### ✅ **(2) Spring Boot + Netty (WebSocket, TCP 서버)**

- HTTP 요청은 Spring Boot에서 처리하고, **WebSocket, TCP 같은 네트워크 통신만 Netty로 처리**.
- **카카오톡 같은 실시간 채팅**을 만들 때 많이 사용됨.

### ✅ **(3) Spring Boot에서 Spring Cloud Gateway 사용 (Netty 내장)**

- Spring Cloud Gateway는 기본적으로 Netty 기반으로 동작하여, **API Gateway로 활용**할 수 있음.

---

## **2. 방법 1: Spring Boot에서 Netty를 내장 서버로 사용 (Spring WebFlux)**

Spring Boot의 기본 내장 서버는 Tomcat이지만, **WebFlux를 사용하면 Netty로 변경 가능**합니다.

### 📌 **1) `pom.xml`에서 Spring WebFlux 추가**


```xml
<dependencies>
    <!-- Spring WebFlux (Netty 사용) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-webflux</artifactId>
    </dependency>
</dependencies>
```


📌 **2) `application.properties`에서 Tomcat 제거**

```properties
spring.main.web-application-type=reactive
```


이렇게 하면 **Spring Boot가 자동으로 Tomcat 대신 Netty를 내장 서버로 사용**합니다.

### 📌 **3) 간단한 WebFlux 컨트롤러 추가**


```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

@RestController
public class HelloController {
    @GetMapping("/hello")
    public Mono<String> sayHello(@RequestParam(defaultValue = "World") String name) {
        return Mono.just("Hello, " + name + "!");
    }
}
```


**🚀 실행 후 `http://localhost:8080/hello?name=Netty`로 접속하면 Netty 서버에서 응답을 줌.**

---

## **3. 방법 2: Spring Boot에서 Netty를 별도로 사용 (WebSocket 채팅 서버)**

Spring Boot의 웹 서버는 Tomcat을 유지하면서, **WebSocket 서버만 Netty로 따로 구현할 수도 있음.**

### 📌 **1) `pom.xml`에 Netty 추가**


```xml
<dependency>
    <groupId>io.netty</groupId>
    <artifactId>netty-all</artifactId>
    <version>4.1.100.Final</version>
</dependency>
```

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.websocketx.*;

public class NettyWebSocketServer {
    private final int port;

    public NettyWebSocketServer(int port) {
        this.port = port;
    }

    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new HttpServerCodec());
                        ch.pipeline().addLast(new HttpObjectAggregator(65536));
                        ch.pipeline().addLast(new WebSocketServerProtocolHandler("/ws"));
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<TextWebSocketFrame>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, TextWebSocketFrame msg) {
                                System.out.println("Received: " + msg.text());
                                ctx.writeAndFlush(new TextWebSocketFrame("Server received: " + msg.text()));
                            }
                        });
                    }
                });

            ChannelFuture f = b.bind(port).sync();
            System.out.println("Netty WebSocket Server started on port: " + port);
            f.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        new NettyWebSocketServer(8081).run();
    }
}
```


**🚀 실행 후 `ws://localhost:8081/ws`로 WebSocket 연결 가능!**

---

## **4. 방법 3: Spring Cloud Gateway에서 Netty 사용**

Spring Cloud Gateway는 기본적으로 Netty를 사용하여 API Gateway를 구축할 수 있습니다.

### 📌 **1) `pom.xml`에 Spring Cloud Gateway 추가**


```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```


📌 **2) `application.yml`에서 API Gateway 설정**


```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: example-service
          uri: http://example.com
          predicates:
            - Path=/api/**
```


이제 `/api/`로 들어오는 요청을 **Netty 기반 Gateway**가 처리합니다.

## **5. 결론**

✅ **Spring Boot + Netty를 같이 사용할 수 있음**  
✅ **Netty를 전체 웹 서버로 사용하려면?** → `Spring WebFlux`  
✅ **WebSocket 같은 실시간 서버만 Netty로 사용하려면?** → Netty 별도 실행  
✅ **Spring Cloud Gateway를 활용하면 API Gateway도 Netty로 동작 가능**



## **1. Spring Legacy에서 Netty를 활용하는 방법**

### ✅ **(1) Spring MVC는 유지하고, WebSocket이나 TCP 서버만 Netty로 따로 실행** 

- Spring MVC는 기존 Tomcat을 유지하고, **WebSocket, TCP 서버**만 Netty로 실행.
- **실시간 채팅, 게임 서버, IoT 통신** 같은 경우 유용.

### ✅ **(2) Spring MVC를 Netty로 실행 (Spring Boot 없이 Netty 사용)**

- **Spring MVC를 Netty 기반의 내장 서버에서 실행**할 수도 있지만, 설정이 복잡해서 권장되지 않음.

### ✅ **(3) Spring Cloud Gateway와 연동 (API Gateway로 Netty 사용)**

- API Gateway를 Netty 기반으로 실행하고, Spring MVC는 별도의 서버로 운영.

---

## **2. 방법 1: Spring MVC에서 Netty WebSocket 서버 실행하기 (Spring MVC + Netty)**

기존 Spring MVC에서 **WebSocket 기반 실시간 채팅 서버**를 Netty로 만들 수 있습니다.

### 📌 **1) `pom.xml`에 Netty 라이브러리 추가**

Spring Legacy(MVC) 프로젝트에서는 Netty를 직접 추가해야 합니다.


```xml
<dependencies>
    <!-- Netty -->
    <dependency>
        <groupId>io.netty</groupId>
        <artifactId>netty-all</artifactId>
        <version>4.1.100.Final</version>
    </dependency>

    <!-- SLF4J (로그) -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.36</version>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.36</version>
    </dependency>
</dependencies>
```


📌 **2) WebSocket Netty 서버 구현 (`NettyWebSocketServer.java`)**

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.websocketx.*;

public class NettyWebSocketServer {
    private final int port;

    public NettyWebSocketServer(int port) {
        this.port = port;
    }

    public void run() throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap b = new ServerBootstrap();
            b.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new HttpServerCodec());
                        ch.pipeline().addLast(new HttpObjectAggregator(65536));
                        ch.pipeline().addLast(new WebSocketServerProtocolHandler("/ws"));
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<TextWebSocketFrame>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, TextWebSocketFrame msg) {
                                System.out.println("Received: " + msg.text());
                                ctx.writeAndFlush(new TextWebSocketFrame("Server received: " + msg.text()));
                            }
                        });
                    }
                });

            ChannelFuture f = b.bind(port).sync();
            System.out.println("Netty WebSocket Server started on port: " + port);
            f.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }

    public static void main(String[] args) throws Exception {
        new NettyWebSocketServer(8081).run();
    }
}
```

✅ **Spring MVC는 기존 Tomcat을 유지하면서, WebSocket 통신만 Netty로 실행**하는 방식입니다.

**🚀 실행 후 `ws://localhost:8081/ws`로 WebSocket 연결 가능!**  
**(Spring MVC는 기존 서버에서 계속 동작함)**

---

## **3. 방법 2: Spring Legacy를 Netty로 실행하기 (Tomcat 제거)**

Spring MVC를 Netty 기반으로 실행할 수도 있지만, **Spring Boot 없이 Netty를 웹 서버로 활용하는 것은 설정이 복잡**합니다.

### 📌 **1) `pom.xml`에 Netty + Spring MVC 라이브러리 추가**


```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.30</version>
    </dependency>

    <dependency>
        <groupId>io.netty</groupId>
        <artifactId>netty-all</artifactId>
        <version>4.1.100.Final</version>
    </dependency>
</dependencies>
```


📌 **2) Netty로 Spring MVC 실행 (`NettySpringServer.java`)**

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.*;

public class NettySpringServer {
    public static void main(String[] args) throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap bootstrap = new ServerBootstrap();
            bootstrap.group(bossGroup, workerGroup)
                .channel(NioServerSocketChannel.class)
                .childHandler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new HttpServerCodec());
                        ch.pipeline().addLast(new HttpObjectAggregator(65536));
                        ch.pipeline().addLast(new SimpleChannelInboundHandler<FullHttpRequest>() {
                            @Override
                            protected void channelRead0(ChannelHandlerContext ctx, FullHttpRequest req) {
                                FullHttpResponse response = new DefaultFullHttpResponse(
                                        req.protocolVersion(), HttpResponseStatus.OK);
                                ctx.writeAndFlush(response);
                            }
                        });
                    }
                });

            ChannelFuture f = bootstrap.bind(8080).sync();
            System.out.println("Spring MVC running on Netty!");
            f.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }
}
```


✅ **Spring MVC를 Netty에서 실행하는 방법이지만, Spring Boot 없이 구현하면 설정이 복잡**해서 추천하지 않습니다.

---

## **4. 결론**

✅ **Spring Legacy(Spring MVC)에서도 Netty를 사용할 수 있음**  
✅ **Tomcat을 유지하면서 WebSocket/TCP만 Netty로 따로 실행하는 것이 가장 현실적인 방법**  
✅ **Spring Boot가 아니라면, Spring MVC 자체를 Netty로 실행하는 것은 어렵고 비효율적**

💡 **결론적으로, Spring MVC는 유지하면서 WebSocket/TCP 서버만 Netty로 실행하는 것이 가장 좋은 방법입니다!** 🚀


### **Spring Legacy(Spring MVC) + Netty에서 오프라인 메시지 저장 기능 구현하기**

✅ **맞습니다! 카카오톡처럼 사용자가 오프라인이어도 메시지를 보내고, 나중에 접속했을 때 받을 수 있도록 해야 합니다.**  
이를 위해 **Redis나 데이터베이스(DB)를 활용하여 오프라인 메시지를 저장하고, 접속 시 전달하는 기능**을 추가하면 됩니다.

---

## **1. 오프라인 메시지 저장을 위한 핵심 원리**

1. **사용자가 접속 중인지 확인 (WebSocket 연결 여부 확인)**
2. **접속 중이라면?** → **바로 메시지 전송**
3. **접속 중이 아니라면?** → **메시지를 Redis 또는 DB에 저장**
4. **사용자가 다시 접속하면?** → **저장된 메시지를 가져와 전달 후 삭제**


```xml
<dependencies>
    <!-- Netty -->
    <dependency>
        <groupId>io.netty</groupId>
        <artifactId>netty-all</artifactId>
        <version>4.1.100.Final</version>
    </dependency>

    <!-- Redis (Jedis) -->
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>4.3.1</version>
    </dependency>

    <!-- SLF4J (로그) -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.36</version>
    </dependency>

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.36</version>
    </dependency>
</dependencies>
```

✅ **2) Netty WebSocket 서버 구현 (`NettyWebSocketServer.java`)**

```java
import io.netty.bootstrap.ServerBootstrap;
import io.netty.channel.*;
import io.netty.channel.nio.NioEventLoopGroup;
import io.netty.channel.socket.nio.NioServerSocketChannel;
import io.netty.channel.socket.SocketChannel;
import io.netty.handler.codec.http.HttpObjectAggregator;
import io.netty.handler.codec.http.HttpServerCodec;
import io.netty.handler.codec.http.websocketx.*;
import redis.clients.jedis.Jedis;

import java.util.concurrent.ConcurrentHashMap;

public class NettyWebSocketServer {
    private static final int PORT = 8081;
    private static final ConcurrentHashMap<String, Channel> clients = new ConcurrentHashMap<>();
    private static final Jedis redis = new Jedis("localhost", 6379); // Redis 연결

    public static void main(String[] args) throws Exception {
        EventLoopGroup bossGroup = new NioEventLoopGroup();
        EventLoopGroup workerGroup = new NioEventLoopGroup();

        try {
            ServerBootstrap bootstrap = new ServerBootstrap();
            bootstrap.group(bossGroup, workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) {
                            ch.pipeline().addLast(new HttpServerCodec());
                            ch.pipeline().addLast(new HttpObjectAggregator(65536));
                            ch.pipeline().addLast(new WebSocketServerProtocolHandler("/ws"));
                            ch.pipeline().addLast(new WebSocketServerHandler());
                        }
                    });

            ChannelFuture future = bootstrap.bind(PORT).sync();
            System.out.println("Netty WebSocket 1:1 Chat Server started on port: " + PORT);
            future.channel().closeFuture().sync();
        } finally {
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
            redis.close();
        }
    }

    static class WebSocketServerHandler extends SimpleChannelInboundHandler<TextWebSocketFrame> {
        @Override
        public void handlerAdded(ChannelHandlerContext ctx) {
            System.out.println("New client connected: " + ctx.channel().id().asShortText());
        }

        @Override
        public void handlerRemoved(ChannelHandlerContext ctx) {
            clients.values().remove(ctx.channel());
            System.out.println("Client disconnected: " + ctx.channel().remoteAddress());
        }

        @Override
        protected void channelRead0(ChannelHandlerContext ctx, TextWebSocketFrame msg) {
            String message = msg.text();
            System.out.println("Received: " + message);

            String[] parts = message.split(":", 3); // "CONNECT:사용자ID" or "MSG:받는사람ID:내용"
            if (parts.length < 2) return;

            String command = parts[0];
            String senderOrReceiver = parts[1];

            if ("CONNECT".equals(command)) {
                clients.put(senderOrReceiver, ctx.channel()); // 사용자 ID와 WebSocket 채널 저장
                ctx.channel().writeAndFlush(new TextWebSocketFrame("CONNECTED AS " + senderOrReceiver));

                // 저장된 오프라인 메시지 전달 후 삭제
                while (redis.llen("offline:" + senderOrReceiver) > 0) {
                    String offlineMessage = redis.lpop("offline:" + senderOrReceiver);
                    ctx.channel().writeAndFlush(new TextWebSocketFrame("Offline Message: " + offlineMessage));
                }
            } else if ("MSG".equals(command) && parts.length == 3) {
                String receiverId = parts[1];
                String chatMessage = parts[2];

                Channel receiverChannel = clients.get(receiverId);
                if (receiverChannel != null) {
                    receiverChannel.writeAndFlush(new TextWebSocketFrame("From " + senderOrReceiver + ": " + chatMessage));
                } else {
                    // 오프라인이면 Redis에 메시지 저장
                    redis.rpush("offline:" + receiverId, senderOrReceiver + ": " + chatMessage);
                }
            }
        }
    }
}
```

3. 클라이언트(JavaScript)에서 WebSocket 연결


```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Netty 1:1 WebSocket Chat</title>
</head>
<body>
    <h2>1:1 채팅</h2>
    <label>내 ID:</label>
    <input type="text" id="userId" placeholder="사용자 ID 입력">
    <button onclick="connect()">연결</button>
    <hr>
    
    <label>받는 사람 ID:</label>
    <input type="text" id="receiverId" placeholder="받는 사람 ID">
    <label>메시지:</label>
    <input type="text" id="message" placeholder="메시지를 입력하세요">
    <button onclick="sendMessage()">전송</button>
    
    <h3>채팅 로그</h3>
    <ul id="messages"></ul>

    <script>
        let socket;
        let userId;

        function connect() {
            userId = document.getElementById("userId").value;
            if (!userId) {
                alert("사용자 ID를 입력하세요.");
                return;
            }

            socket = new WebSocket("ws://localhost:8081/ws");

            socket.onopen = function () {
                console.log("WebSocket 연결됨!");
                socket.send("CONNECT:" + userId);
            };

            socket.onmessage = function (event) {
                let msgList = document.getElementById("messages");
                let msgItem = document.createElement("li");
                msgItem.textContent = event.data;
                msgList.appendChild(msgItem);
            };
        }

        function sendMessage() {
            let receiverId = document.getElementById("receiverId").value;
            let message = document.getElementById("message").value;
            socket.send("MSG:" + receiverId + ":" + message);
        }
    </script>
</body>
</html>
```

## **4. 결론**

✅ **카톡처럼 사용자가 오프라인이어도 메시지 저장 후 접속하면 받을 수 있음**  
✅ **Redis를 활용하여 오프라인 메시지를 저장하고, 로그인 시 전달**  
✅ **이제 Netty 기반으로 실제 채팅 앱처럼 동작 가능!**

💡 **이제 Spring MVC + Netty로 오프라인 메시지를 처리할 수 있습니다! 🚀**






---
출처 - https://soonmin.tistory.com/71