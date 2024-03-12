
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

        
		// 세션의 최대 미활동 시간을 TIME(초 단위)로 설정
        request.getSession().setMaxInactiveInterval(TIME);  

		// 상위 클래스의 onAuthenticationSuccess 메소드를 호출하여 기본 로그인 성공 처리를 실행
		super.onAuthenticationSuccess(request, response, auth);
    }
}
```


**`super.onAuthenticationSuccess(request, response, auth);` 호출 시 수행되는 작업**

`super.onAuthenticationSuccess(request, response, auth);`를 호출하면 로그인 성공 후에 상위 클래스인 `AuthenticationSuccessHandler`의 `onAuthenticationSuccess` 메서드를 실행합니다. 이 메서드는 사용자 인증에 성공했을 때 필요한 로직을 처리하는 역할을 합니다.

### `AuthenticationSuccessHandler`의 역할

- **리디렉션 제어**: 로그인 후 사용자를 특정 페이지로 안내합니다. 이는 리디렉션 또는 포워딩을 통해 수행될 수 있습니다.
- **사용자 경험 개선**: 추가 정보를 로딩하거나, 로그인 성공 알림 메시지를 표시하는 등 사용자 경험이 향상됩니다.

Spring Security에서는 `AuthenticationSuccessHandler`의 기본 구현을 제공하여, 개발자가 별도로 구현하지 않아도 일반적인 로그인 성공 시의 동작을 지원합니다. 이 기본 구현을 통해 로그인 성공 후 필요한 리디렉션이나 다른 동작을 수행할 수 있습니다.

- **SavedRequestAwareAuthenticationSuccessHandler**: 사용자가 인증되기 전에 요청했던 URL로 리디렉션합니다. 로그인 페이지로 리디렉션되기 전에 접근하려고 했던 페이지가 있다면, 로그인 성공 후 그 페이지로 자동으로 리디렉션되는 기능을 제공합니다.
- **SimpleUrlAuthenticationSuccessHandler**: 특정 URL로 리디렉션하는 기능을 제공합니다. 개발자는 로그인 성공 시 사용자를 리디렉션하고자 하는 URL을 설정할 수 있습니다.


### 사용 방법

`spring-security.xml` 설정 파일 내에 `<form-login>` 태그의 `default-target-url`속성을 설정함으로써 `SimpleUrlAuthenticationSuccessHandler`를 사용할 수 있으며, 별도의 `AuthenticationSuccessHandler` 구현체를 지정하지 않는 한, 기본적으로 `SavedRequestAwareAuthenticationSuccessHandler`가 사용됩니다.

---

개발자가 별도로 `AuthenticationSuccessHandler`를 구현하고자 할 경우, 이러한 기본 제공 구현체를 직접 사용하거나, 상속 받아 필요한 비즈니스 로직을 추가할 수 있습니다. 이를 통해 로그인 프로세스를 더욱 세밀하게 제어할 수 있습니다


로그인 실패

```java
public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response,  
       AuthenticationException exception) throws IOException, ServletException {  
    setDefaultFailureUrl("/login?error=true");  
    super.onAuthenticationFailure(request, response, exception);  
}
```


Spring Security에서 로그인 실패 시의 행동도 개발자가 별도로 구현하지 않아도 기본적인 동작을 제공합니다. `setDefaultFailureUrl("/login?error=true")`는 로그인 실패 시 사용자를 특정 URL로 리디렉션하는 설정이며, `super.onAuthenticationFailure(request, response, exception);` 호출을 통해 제공되는 기본 로직을 실행하면 됩니다.

- **SimpleUrlAuthenticationFailureHandler**: 로그인 실패 시 지정된 URL로 리디렉션합니다. 개발자는 로그인 실패 시 사용자를 안내하고자 하는 URL을 설정할 수 있습니다.

**`super.onAuthenticationFailure(request, response, exception);`의 역할과 구현**

`super.onAuthenticationFailure(request, response, exception);`는 Spring Security의 `AuthenticationFailureHandler` 인터페이스를 통해 로그인 실패 시 처리 로직을 정의하는 부분입니다. 기본적으로, 이 메소드는 로그인 시도가 실패했을 때 수행될 기본 동작을 실행하는데, 이는 상속받은 핸들러(`SimpleUrlAuthenticationFailureHandler` 등)의 로그인 실패 처리 로직을 호출합니다. 따라서, 별도로 구현한 로직이 없다면, Spring Security의 기본적인 실패 처리가 수행됩니