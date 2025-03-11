
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


```java

```