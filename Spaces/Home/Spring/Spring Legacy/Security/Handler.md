
`HandlerInterceptorAdapter`는 스프링(Spring) 프레임워크에서 제공하는 클래스로, 웹 요청을 가로채고 특정 로직을 실행할 수 있게 해주는 인터셉터의 기본 구현체입니다. 사용자가 로그인했는지 체크하거나, 로깅을 위한 처리, 공통적으로 요청에 대한 전처리나 후처리를 하고 싶을 때 유용하게 사용됩니다. 🌟

**사용되는 이유**:

1. **중복 제거**: 여러 컨트롤러에 걸쳐서 공통으로 수행되어야 하는 작업을 중앙에서 관리할 수 있어서 코드 중복을 줄일 수 있습니다.
2. **효율성**: 특정 조건에 따라 요청을 미리 차단하거나 처리함으로써 시스템의 효율성을 높일 수 있습니다.




```java
import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyInterceptor extends HandlerInterceptorAdapter {

    // 컨트롤러 메서드가 호출되기 전에 실행됩니다.
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        // 여기에 전처리 로직을 구현합니다.
        // 예) 사용자 인증 체크
        return true; // true면 컨트롤러 메서드로 계속 진행, false면 중단
    }

    // 컨트롤러 메서드 호출 후, 화면(View) 렌더링 전에 실행됩니다.
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
        // 여기에 후처리 로직을 구현합니다.
        // 예) 공통 모델 데이터 추가
    }

    // 완전히 끝난 후에 실행됩니다. (뷰 렌더링까지 완료된 후)
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        // 예) 리소스 해제
    }
}


```

로그인 성공

```java
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;

public class MyAuthenticationSuccessHandler 
  implements AuthenticationSuccessHandler {
    
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, 
      HttpServletResponse response, Authentication authentication) 
      throws IOException, ServletException {
        
        // 로그인 성공 후 처리 로직
        // 예: 홈페이지로 리디렉션
        response.sendRedirect("/homepage");
    }
}

```

로그인 실패

```java
import org.springframework.security.web.authentication.AuthenticationFailureHandler;

public class MyAuthenticationFailureHandler 
  implements AuthenticationFailureHandler {
    
    @Override
    public void onAuthenticationFailure(HttpServletRequest request, 
      HttpServletResponse response, AuthenticationException exception) 
      throws IOException, ServletException {
        
        // 로그인 실패 후 처리 로직
        // 예: 로그인 페이지로 재이동하며 에러 메시지 전달
        request.setAttribute("loginError", "true");
        request.getRequestDispatcher("/login").forward(request, response);
    }
}

```


로그인 성공시 

```java
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;

public class MyAuthenticationSuccessHandler 
  implements AuthenticationSuccessHandler {
    
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, 
      HttpServletResponse response, Authentication authentication) 
      throws IOException, ServletException {
        
        // 로그인 성공 후 처리 로직
        // 예: 홈페이지로 리디렉션
        
		// 세션의 최대 미활동 시간을 TIME(초 단위)로 설정
        request.getSession().setMaxInactiveInterval(TIME);  

		
		super.onAuthenticationSuccess(request, response, auth);
    }
}
```
