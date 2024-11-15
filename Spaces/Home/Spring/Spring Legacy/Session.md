

```java
import javax.servlet.http.HttpSession;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

public class SessionTimeoutListener implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent event) {
        // 세션 생성 시 아무 작업도 하지 않음
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent event) {
        HttpSession session = event.getSession();

        // 세션 만료 시, 로그에 기록
        session.getServletContext().log("세션 만료: " + session.getId());

        // 세션에 세션 만료 플래그를 추가 (클라이언트가 이 정보를 확인할 수 있도록)
        session.setAttribute("sessionExpired", true);
    }
}
```

web.xml
```xml
<listener>
    <listener-class>com.example.SessionTimeoutListener</listener-class>
</listener>
```



`HttpSessionListener`는 **서버 측에서 세션 이벤트를 처리**하는 리스너입니다. `sessionDestroyed` 메서드는 **세션이 종료되거나 만료될 때 호출**됩니다. 하지만 중요한 점은 **`HttpSessionListener`**는 **서버 측에서 발생하는 이벤트**로, 클라이언트 측에서의 동작이나 리디렉션을 처리하는 데는 적합하지 않다는 것입니다.




security-context.xml 에서 http 태그 안에 삽입
```xml
<security:session-management invalid-session-url="/login/login">  
    <security:concurrency-control max-sessions="1" error-if-maximum-exceeded="true" />  
</security:session-management>


<security:session-management invalid-session-url ="/login/login" session-fixation-protection="migrateSession">  
    <security:concurrency-control max-sessions="1" error-if-maximum-exceeded="false" />  
</security:session-management>
```


web.xml
```xml
<session-config>  
    <session-timeout>30</session-timeout>  <!-- 30분 -->  
</session-config>
```




```java
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.support.WebApplicationContextUtils;
import org.springframework.beans.factory.annotation.Autowired;

public class SessionTimeoutListener implements HttpSessionListener {

    private SSEController sseController;

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        // 세션 생성 시 처리
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        // 세션 만료 시 로그 출력
        System.out.println("세션 만료: " + se.getSession().getId());

        // ServletContext에서 WebApplicationContext 가져오기
        ServletContext servletContext = se.getSession().getServletContext();
        WebApplicationContext ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);

        // WebApplicationContext에서 SSEController 빈을 가져옵니다.
        if (sseController == null) {
            sseController = ctx.getBean(SSEController.class);
        }

        // sseController가 null이 아닌지 확인 후 알림 보내기
        if (sseController != null) {
            String sessionId = se.getSession().getId();
            sseController.notifySessionExpired(sessionId);  // 세션 만료 알림 전송
        } else {
            System.out.println("SSEController 빈을 찾을 수 없습니다.");
        }
    }
}

```



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





## 최종 Session 만료 시 알림


SessionListener
```java
package com.kpop.merch.common.filter;  
  
  
import com.kpop.merch.common.controller.SessionSseController;  
import org.springframework.web.context.WebApplicationContext;  
import org.springframework.web.context.support.WebApplicationContextUtils;  
  
import javax.servlet.ServletContext;  
import javax.servlet.http.HttpSession;  
import javax.servlet.http.HttpSessionEvent;  
import javax.servlet.http.HttpSessionListener;  
import java.time.ZoneId;  
import java.time.ZonedDateTime;  
import java.time.format.DateTimeFormatter;  
  
public class SessionTimeoutListener implements HttpSessionListener {  
  
  
    private SessionSseController sseController;  
  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        HttpSession session = se.getSession();  
        System.out.println("세션 생성: " + session.getId());  
        session.getServletContext().log("세션 생성: " + session.getId());  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        HttpSession session = se.getSession();  
        session.getServletContext().log("세션 만료: " + session.getId());  
  
        System.out.println("세션 만료: " + session.getId() + " 현재 시간: " +  
            ZonedDateTime.now(ZoneId.of("Asia/Seoul")).format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));  
  
  
        // ServletContext에서 WebApplicationContext 가져오기  
        ServletContext servletContext = se.getSession().getServletContext();  
        WebApplicationContext ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);  
  
        // WebApplicationContext에서 SSEController 빈을 가져옵니다.  
        if (sseController == null) {  
            sseController = ctx.getBean(SessionSseController.class);  
        }  
        sseController.notifySessionExpired();  
    }  
}
```


SSEController
```java
package com.kpop.merch.common.controller;  
  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RestController;  
import org.springframework.web.servlet.mvc.method.annotation.SseEmitter;  
  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
import java.util.List;  
import java.util.concurrent.CopyOnWriteArrayList;  
  
@RestController  
public class SessionSseController {  
  
    private static final List<SseEmitter> emitters = new CopyOnWriteArrayList<>();  
  
    @Autowired  
    private HttpSession session;  // 세션을 클래스 필드로 선언  
  
  
    @GetMapping("/sse/session")  
    public SseEmitter handleSse(HttpSession session) {  
        if (session == null || session.getAttribute("user") == null) {  
            System.out.println("세션 없음 또는 유효하지 않은 사용자");  
            return null; // 또는 에러 응답 반환  
        }  
  
        // 새로운 SseEmitter 객체 생성  
        SseEmitter emitter = new SseEmitter(0L);  // 0L은 무제한 대기  
        // emitter가 리스트에 정상적으로 추가되는지 확인  
        emitters.add(emitter);  
        System.out.println("새로운 emitter 추가됨. 현재 emitters 크기: " + emitters.size());  
  
        emitter.onCompletion(() -> {  
            System.out.println("SseEmitter 완료됨, 리스트에서 제거: " + emitter);  
            emitters.remove(emitter);  
        });  
  
        emitter.onTimeout(() -> {  
            System.out.println("SseEmitter 타임아웃 발생, 리스트에서 제거: " + emitter);  
            emitters.remove(emitter);  
        });  
        // 클라이언트에서 연결이 성공적으로 이루어졌는지 확인하는 로그  
  
  
        return emitter;  
    }  
  
    public void notifySessionExpired() {  
        System.out.println("세션 만료 알림 시작");  
  
        // emitters 리스트 상태 확인  
        System.out.println("현재 emitters 크기: " + emitters.size());  
  
        Boolean isLoggedOut = (Boolean) session.getAttribute("logoutFlag");  
        if (isLoggedOut != null && isLoggedOut) {  
            System.out.println("사용자가 직접 로그아웃함. 세션 만료 알림 전송하지 않음.");  
            return;  
        }  
  
        for (SseEmitter emitter : emitters) {  
            System.out.println("emitters 루프 시작");  
  
  
            try {  
                emitter.send(SseEmitter.event().data("세션이 만료되었습니다."));  
                emitter.complete();  
            } catch (IOException e) {  
                e.printStackTrace();  // 예외 출력  
                emitters.remove(emitter);  // 오류가 나면 해당 emitter 제거  
            }  
        }  
    }  
}
```


js 코드
```js

let eventSource;  
  
function connectSSE() {  
    eventSource = new EventSource("/sse/session");  
  
    eventSource.onopen = function(event) {  
        console.log("SSE 연결이 성공적으로 이루어졌습니다.");  
    };  
  
    eventSource.onmessage = function(event) {  
        console.log("서버로부터 받은 메시지:", event.data);  
  
        if (event.data === "세션이 만료되었습니다.") {  
            alert(event.data);  
            eventSource.close();  // 연결 종료  
            window.location.href = "/login/login";  // 로그인 페이지로 리다이렉션  
        }  
    };  
  
    eventSource.onerror = function(event) {  
        console.error("SSE 연결이 끊어졌습니다.");  
        if (eventSource.readyState === EventSource.CLOSED) {  
            setTimeout(connectSSE, 3000);  // 3초 후 재연결  
        }  
    };  
}  
  
// 페이지 로드 시 SSE 연결 초기화  
connectSSE();
```


LogoutSuccessHandler
```java
package com.kpop.merch.login.handler;  
  
import org.springframework.security.core.Authentication;  
import org.springframework.security.web.authentication.logout.LogoutSuccessHandler;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
  
public class CustomLogoutSuccessHandler implements LogoutSuccessHandler {  
  
    @Override  
    public void onLogoutSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {  
        // 로그아웃 성공 후 처리할 작업  
        System.out.println("로그아웃 성공!");  
  
        // 로그아웃 후 /login 페이지로 리다이렉트  
        HttpSession session = request.getSession(false);  
        if (session != null) {  
            session.setAttribute("logoutFlag", true);  // 로그아웃 플래그 설정  
        }  
  
        response.sendRedirect("/login/login");  // 로그인 페이지로 리다이렉트  
    }  
}
```


security-context.xml

```xml
<security:logout  
        logout-url="/login/logout"  
        invalidate-session="true"  
        delete-cookies="JSESSIONID"  
        success-handler-ref="logoutSuccessHandler"  
/>
```


web.xml
listener 추가
```xml
<listener>  
    <listener-class>com.kpop.merch.common.filter.SessionTimeoutListener</listener-class>  
</listener>

```

filter에 `<async-supported>true</async-supported>` 추가 sse 비동기 통신을 하기 위함

```xml
<filter>  
    <filter-name>springSecurityFilterChain</filter-name>  
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
    <async-supported>true</async-supported> <!-- 추가 -->  
</filter>

<servlet>  
  <servlet-name>appServlet</servlet-name>  
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  
  <init-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>/WEB-INF/spring/servlet-context.xml</param-value>  
  </init-param>  
  <load-on-startup>1</load-on-startup>  
  <async-supported>true</async-supported> <!-- 이 줄을 추가 -->  
</servlet>

```

