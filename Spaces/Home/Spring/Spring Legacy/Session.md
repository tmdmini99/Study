

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
