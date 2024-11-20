

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

```java
private static final List<SseEmitter> emitters = new CopyOnWriteArrayList<>();  
```

여기서 static으로 설정해줘야함

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


프론트 타이머
```js
// // 세션 만료 시간을 설정 (단위: 밀리초)  
// const sessionExpireTime = 30 * 60 * 1000; // 30분  
// const alertBeforeExpire = 5 * 60 * 1000;  // 5분 전에 알림  
//  
// // 경고 타이머 설정  
// const alertTimer = setTimeout(() => {  
//   alert("세션이 5분 후에 만료됩니다. 계속하려면 세션을 연장하세요.");  
// }, sessionExpireTime - alertBeforeExpire);  
//  
// // 실제 만료 시점에 알림 또는 세션 종료 처리  
// const expireTimer = setTimeout(() => {  
//   alert("세션이 만료되었습니다. 다시 로그인 해주세요.");  
//   // 로그아웃 처리나 페이지 리다이렉트 등의 추가 작업 가능  
// }, sessionExpireTime);  
  
// let eventSource;  
//  
// function connectSSE() {  
//     // SSE 연결을 시작  
//     eventSource = new EventSource("/sse/session");  
//  
//     // 연결이 성공했을 때 이벤트 처리  
//     eventSource.onopen = function(event) {  
//         console.log("SSE 연결이 성공적으로 이루어졌습니다.");  
//     };  
//  
//     // 서버에서 보낸 메시지 처리  
//     eventSource.onmessage = function(event) {  
//         console.log("서버로부터 받은 메시지:", event.data);  
//  
//         if (event.data === "세션이 만료되었습니다.") {  
//             alert(event.data);  
//             // 세션 만료 시 연결 종료  
//             eventSource.close();  // EventSource 종료  
//             window.location.href = "/login";  // 로그인 페이지로 리다이렉션  
//         }  
//     };  
//  
//     eventSource.onerror = function(event) {  
//         console.error("SSE 연결이 끊어졌습니다.");  
//         eventSource.close();  
//     };  
// }  
//  
// // SSE 연결 초기화  
// connectSSE();
```



security.xml

```xml
<security:http auto-config="true" use-expressions="true">  
    <security:custom-filter before="LOGOUT_FILTER" ref="customLogoutFilter"/>  
        <security:headers>  
            <security:frame-options policy="SAMEORIGIN" />  
            <security:frame-options disabled="true" />  
            <security:content-security-policy policy-directives="frame-ancestors *;" />  
  
        </security:headers>  
  
        <security:custom-filter ref="commonFilter" after="SESSION_MANAGEMENT_FILTER" />  
  
        <security:intercept-url pattern="/resources/**" access="permitAll()"/>  
        <security:intercept-url pattern="/events/**" access="permitAll()" />  
<!--        <security:intercept-url pattern="/" access="permitAll()" />-->  
        <security:intercept-url pattern="/login/login" access="permitAll()" />  
        <security:intercept-url pattern="/**" access="permitAll()" />  
  
        <security:form-login  
            login-processing-url="/login/login"  
            authentication-failure-url="/login/login?error=true"  
            login-page="/login/login"  
            username-parameter="username"  
            password-parameter="password"  
            default-target-url="/"  
            authentication-success-handler-ref="loginSuccessHandler" />  
  
        <security:logout  
                logout-url="/login/logout"  
                invalidate-session="true"  
                delete-cookies="JSESSIONID"  
                success-handler-ref="logoutSuccessHandler"  
        />  
        <!-- 로그인 후 세션을 생성하도록 설정 -->  
  
        <security:session-management invalid-session-url ="/login/login" session-fixation-protection="migrateSession">  
            <security:concurrency-control max-sessions="1" error-if-maximum-exceeded="false" />  
        </security:session-management>  
    </security:http>
```

여기에 등록

```xml
<security:custom-filter before="LOGOUT_FILTER" ref="customLogoutFilter"/>  
```





```java
package com.kpop.merch.login.handler;  
  
import org.springframework.security.core.Authentication;  
import org.springframework.security.web.authentication.logout.LogoutHandler;  
  
import javax.servlet.*;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
  
public class CustomLogoutFilter implements Filter {  
  
  
    @Override  
    public void init(FilterConfig filterConfig) throws ServletException {  
  
    }  
    @Override  
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {  
        HttpServletRequest httpRequest = (HttpServletRequest) request;  
        HttpServletResponse httpResponse = (HttpServletResponse) response;  
        HttpSession session = httpRequest.getSession(false);  
        if (session != null && httpRequest.getRequestURI().endsWith("/login/logout")) {  
            System.out.println("CustomLogoutFilter 호출됨2");  
            session.setAttribute("logout", true);  
        }  
        chain.doFilter(request, response);  
    }  
  
    @Override  
    public void destroy() {  
  
    }}
```



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
        // 세션에서 isLogout 값 가져오기  
        Boolean isLogout = (Boolean) session.getAttribute("logout");  
        if (!(isLogout != null && isLogout)) {  
            sseController.notifySessionExpired();  
        }  
    }  
}
```


??
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
  
        // 기존 세션 ID를 ServletContext에 저장하는 대신, 세션 속성으로 저장  
        ServletContext servletContext = session.getServletContext();  
        servletContext.setAttribute("existingSessionId", session.getId());  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        HttpSession session = se.getSession();  
        session.getServletContext().log("세션 만료: " + session.getId());  
  
        System.out.println("세션 만료: " + session.getId() + " 현재 시간: " +  
                ZonedDateTime.now(ZoneId.of("Asia/Seoul")).format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));  
  
        // ServletContext에서 WebApplicationContext 가져오기  
        ServletContext servletContext = session.getServletContext();  
        WebApplicationContext ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);  
  
        if (sseController == null) {  
            sseController = ctx.getBean(SessionSseController.class);  
        }  
  
        // 세션에서 'logout' 값 가져오기 (로그아웃 여부 확인)  
        Boolean isLogout = (Boolean) session.getAttribute("logout");  
  
        // 세션에서 'newLogin' 값 가져오기 (새 로그인 여부 확인)  
        Boolean newLogin = (Boolean) session.getAttribute("newLogin");  
  
        // 기존 세션과 새로운 세션을 비교  
        String existingSessionId = (String) servletContext.getAttribute("existingSessionId");  
  
        if (existingSessionId != null && !existingSessionId.equals(session.getId())) {  
            // 중복 로그인 발생 시 기존 세션에만 알림 전송  
            System.out.println("중복 로그인 발생: 기존 사용자에게만 알림 전송");  
  
            // 중복 로그인 알림  
//            sseController.notifySessionExpired2();  // 중복 로그인 알림 전송  
  
            // 세션 만료 알림을 보내지 않도록 처리  
            return; // 중복 로그인 알림을 보내고 세션 만료 알림을 보내지 않음  
        }  
  
        if (Boolean.TRUE.equals(isLogout)) {  
            // 로그아웃 처리된 경우 알림을 보내지 않음  
            System.out.println("로그아웃된 세션: 알림 전송 안 함");  
            return;  
        }  
  
        // 세션 만료: 해당 사용자에게만 알림  
        System.out.println("세션 만료: 현재 사용자에게 알림 전송");  
        sseController.notifySessionExpired(); // 해당 세션에만 세션 만료 알림  
    }  
  
  
}
```



#### **`HttpSessionBindingListener` 활용**

`HttpSessionBindingListener`를 사용하면 속성이 세션에 추가되거나 제거될 때 이벤트를 처리할 수 있습니다. 이를 통해 `userId`가 세션에 추가될 때 `HttpSessionListener`처럼 동작하도록 구현할 수 있습니다.



```java
package com.kpop.merch.common.filter;  
  
import javax.servlet.ServletContext;  
import javax.servlet.http.HttpSession;  
import javax.servlet.http.HttpSessionBindingEvent;  
import javax.servlet.http.HttpSessionBindingListener;  
import java.util.Collection;  
import java.util.Enumeration;  
import java.util.concurrent.ConcurrentHashMap;  
  
public class SessionAttributeListener implements HttpSessionBindingListener {  
  
    private static final ConcurrentHashMap<HttpSession, String> loginUsers = new ConcurrentHashMap<>();  
       private static SessionAttributeListener sessionListener = null;  
  
       // 싱글톤 객체를 반환하는 메소드  
       public static synchronized SessionAttributeListener getInstance() {  
           if (sessionListener == null) {  
               sessionListener = new SessionAttributeListener();  
           }  
           return sessionListener;  
       }  
  
       // 세션이 연결될 때 호출 (해시맵에 접속자 저장)  
       @Override  
       public void valueBound(HttpSessionBindingEvent event) {  
           // userId가 세션에 바인딩될 때 호출  
           loginUsers.put(event.getSession(), event.getName());  
           System.out.println(event.getName() + " 로그인 완료");  
           System.out.println("현재 접속자 수 : " + getUserCount());  
       }  
  
       // 세션이 끊겼을 때 호출  
       @Override  
       public void valueUnbound(HttpSessionBindingEvent event) {  
           // userId가 세션에서 언바인딩될 때 호출  
           loginUsers.remove(event.getSession());  
           System.out.println(event.getName() + " 로그아웃 완료");  
           System.out.println("현재 접속자 수 : " + getUserCount());  
       }  
  
       // 특정 userId의 세션을 찾아서 무효화 (로그아웃 처리)  
       public void removeSession(String userId) {  
           Enumeration<HttpSession> e = loginUsers.keys();  
           while (e.hasMoreElements()) {  
               HttpSession session = e.nextElement();  
               if (loginUsers.get(session).equals(userId)) {  
                   session.invalidate(); // 세션 무효화  
               }  
           }  
       }  
  
       // 현재 접속자 중 해당 아이디가 사용중인지를 체크  
       public boolean isUsing(String userId) {  
           return loginUsers.containsValue(userId);  
       }  
  
       // 로그인 완료된 사용자의 아이디를 세션에 저장하는 메소드  
       public void setSession(HttpSession session, String userId) {  
           // SessionBinding 이벤트 발생  
           session.setAttribute(userId, this); // 세션에 사용자 아이디와 함께 SessionListener 객체를 바인딩  
       }  
  
       // 세션으로부터 userId를 조회  
       public String getUserID(HttpSession session) {  
           return loginUsers.get(session);  
       }  
  
       // 현재 접속자 수 반환  
       public int getUserCount() {  
           return loginUsers.size();  
       }  
  
       // 현재 접속중인 모든 사용자 리스트 출력  
       public void printLoginUsers() {  
           Enumeration<HttpSession> e = loginUsers.keys();  
           int i = 0;  
           System.out.println("===========================================");  
           while (e.hasMoreElements()) {  
               HttpSession session = e.nextElement();  
               System.out.println((++i) + ". 접속자 : " + loginUsers.get(session));  
           }  
           System.out.println("===========================================");  
       }  
  
       // 현재 접속중인 모든 사용자 목록 반환  
       public Collection<String> getUsers() {  
           return loginUsers.values();  
       }  
   }
```


원본 코드
```java
@WebListener
public class SessionConfig implements HttpSessionListener{
	
    private static final Map<String, HttpSession> sessions = new ConcurrentHashMap<>();

    //중복로그인 지우기
    public synchronized static String getSessionidCheck(String type, String compareId){
	String result = "";
	for( String key : sessions.keySet() ){
		HttpSession hs = sessions.get(key);
		Auth auth = new Auth();
		if(hs != null) {
			auth = (Auth) hs.getAttribute(type);
			if(auth != null && auth.getUserId().toString().equals(compareId)) {
				result = key.toString();
			}
		}
	}
	removeSessionForDoubleLogin(result);
	return result;
    }
    
    private static void removeSessionForDoubleLogin(String userId){    	
        System.out.println("remove userId : " + userId);
        if(userId != null && userId.length() > 0){
            sessions.get(userId).invalidate();
            sessions.remove(userId);    		
        }
    }

    @Override
    public void sessionCreated(HttpSessionEvent hse) {
        System.out.println(hse);
        sessions.put(hse.getSession().getId(), hse.getSession());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent hse) {
        if(sessions.get(hse.getSession().getId()) != null){
            sessions.get(hse.getSession().getId()).invalidate();
            sessions.remove(hse.getSession().getId());	
        }
    }
}
```



## 직접 적용 코드

```java
package com.kpop.merch.common.filter;  
  
import com.kpop.merch.common.controller.SessionSseController;  
import org.springframework.web.context.WebApplicationContext;  
import org.springframework.web.context.support.WebApplicationContextUtils;  
  
import javax.servlet.ServletContext;  
import javax.servlet.annotation.WebListener;  
import javax.servlet.http.HttpSession;  
import javax.servlet.http.HttpSessionEvent;  
import javax.servlet.http.HttpSessionListener;  
import java.time.ZoneId;  
import java.time.ZonedDateTime;  
import java.time.format.DateTimeFormatter;  
import java.util.Map;  
import java.util.concurrent.ConcurrentHashMap;  
  
@WebListener  
public class SessionTimeoutListener implements HttpSessionListener {  
  
    // 세션 관리 및 중복 로그인 방지를 위한 세션 맵  
    private static final Map<String, HttpSession> sessions = new ConcurrentHashMap<>();  
    private SessionSseController sseController;  
  
    @Override  
    public void sessionCreated(HttpSessionEvent se) {  
        HttpSession session = se.getSession();  
        System.out.println("세션 생성: " + session.getId());  
        session.getServletContext().log("세션 생성: " + session.getId());  
  
        // 로그인 시 중복 로그인 체크  
        String userId = getSessionidCheck("user", (String) session.getAttribute("user"));  
        if (userId != null && !userId.isEmpty()) {  
            // 기존 세션이 존재하면 해당 세션을 무효화  
            System.out.println("중복 로그인 방지: 기존 세션 무효화: " + userId);  
            removeSessionForDoubleLogin(userId);  
        }  
  
        // 세션을 관리 맵에 추가  
        sessions.put(session.getId(), session);  
    }  
  
    @Override  
    public void sessionDestroyed(HttpSessionEvent se) {  
        HttpSession session = se.getSession();  
        session.getServletContext().log("세션 만료: " + session.getId());  
  
        System.out.println("세션 만료: " + session.getId() + " 현재 시간: " +  
                ZonedDateTime.now(ZoneId.of("Asia/Seoul")).format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));  
  
        // WebApplicationContext에서 SessionSseController를 가져오기  
        ServletContext servletContext = session.getServletContext();  
        WebApplicationContext ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext);  
  
        if (sseController == null) {  
            sseController = ctx.getBean(SessionSseController.class);  
        }  
  
        // 세션 속성에서 로그아웃 상태 확인  
        Boolean isLogout = (Boolean) session.getAttribute("logout");  
        String sessionId = session.getId();  
  
        if (Boolean.TRUE.equals(isLogout)) {  
            // 로그아웃된 세션: 알림 전송 안 함  
            System.out.println("로그아웃된 세션: 알림 전송 안 함");  
            removeSessionForDoubleLogin(sessionId);  
            return;  
        }  
  
        // 세션 만료: 해당 세션에만 알림 전송  
        System.out.println("세션 만료: 현재 사용자에게 알림 전송");  
        sseController.notifySessionExpiredForSession(sessionId, "세션이 만료되었습니다.");  
  
        // 세션 종료 시 중복 로그인 방지를 위한 세션 제거  
        removeSessionForDoubleLogin(sessionId);  
    }  
  
    // 중복 로그인 체크 메서드  
    public synchronized static String getSessionidCheck(String type, String compareId) {  
        String result = "";  
        for (String key : sessions.keySet()) {  
            HttpSession hs = sessions.get(key);  
            if (hs != null) {  
                Object auth = hs.getAttribute(type); // "user" 타입으로 로그인 정보 가져옴  
                if (auth != null && auth instanceof String) { // 로그인 정보가 String 형태로 저장되어 있다고 가정  
                    String userId = (String) auth;  
                    if (userId.equals(compareId)) {  
                        result = key; // 중복된 세션 ID 반환  
                    }  
                }  
            }  
        }  
        removeSessionForDoubleLogin(result); // 중복 세션 삭제  
        return result;  
    }  
  
    // 중복 로그인 시 기존 세션 무효화  
    private static void removeSessionForDoubleLogin(String userId) {  
        if (userId != null && userId.length() > 0) {  
            HttpSession session = sessions.get(userId);  
            if (session != null) {  
                session.invalidate(); // 세션 무효화  
                sessions.remove(userId); // 세션 맵에서 삭제  
                System.out.println("중복 로그인 방지: 세션 무효화: " + userId);  
            }  
        }  
    }  
}
```



```java
package com.kpop.merch.login.handler;  
  
import com.kpop.merch.common.filter.SessionTimeoutListener;  
import com.kpop.merch.login.vo.LoginVo;  
import org.springframework.security.core.Authentication;  
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;  
  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import javax.servlet.http.HttpSession;  
import java.io.IOException;  
  
public class LoginSuccessHandler implements AuthenticationSuccessHandler {  
  
    private static final String SESSION_KEY = "user"; // 세션에 저장될 키  
  
    @Override  
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {  
        // 인증된 사용자 정보 가져오기 (LoginVo 객체)  
  
            authentication.getPrincipal(); // LoginVo 객체를 가져옵니다.  
            System.out.println("들어옴0000000000000000000000000000000000000000000000000000000000000000");  
            // 중복 로그인 체크 (중복 로그인 방지 로직 추가)  
            String userId = SessionTimeoutListener.getSessionidCheck("user", authentication.getName()); // userId로 중복 로그인 체크  
  
            if (userId != null && !userId.isEmpty()) {  
                // 기존 세션이 존재하면 해당 세션을 무효화하는 로직을 추가  
                System.out.println("중복 로그인 방지: 기존 세션 무효화: " + userId);  
                // 기존 세션 무효화 처리 추가  
                HttpSession existingSession = request.getSession(false); // 기존 세션을 가져옴  
                if (existingSession != null) {  
                    existingSession.invalidate(); // 기존 세션 무효화  
                }  
            }  
  
            // 새로운 세션 생성 후, 세션에 사용자 정보 저장  
            HttpSession session = request.getSession();  
            session.setAttribute(SESSION_KEY, authentication.getName()); // LoginVo 객체를 세션에 저장  
//            session.setMaxInactiveInterval(1800); // 세션 만료 시간 설정 (예: 30분)  
  
            // 리다이렉트 (홈 화면으로 이동 등)  
            response.sendRedirect("/"); // 로그인 후 홈으로 리다이렉트  
  
    }  
}
```



---
출처 - https://medium.com/@leejungmin03/spring-%EC%A4%91%EB%B3%B5%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EB%B0%A9%EC%A7%80-9ef32f7e7110

https://ikso2000.tistory.com/103