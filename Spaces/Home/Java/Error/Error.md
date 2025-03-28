
error 설정

### 1. **`@ControllerAdvice` + `@ExceptionHandler` 사용**

**500에 대한 처리**는 이렇게:


```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public String handleAllException(Exception ex, HttpServletRequest request) {
        // 로그 찍기 원하면 여기에 추가
        return "redirect:/"; // 메인페이지로 이동
    }
}
```


### 2. **404 같은 "에러 페이지"는 Web 설정에서 따로 지정**

`404`는 **Exception이 아니라 Dispatcher가 못 찾은 URI라서**,  
`@ExceptionHandler`로는 안 잡힘.  
→ 그래서 `WebMvcConfigurer`에 아래 설정 추가해야 돼.


```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        resolvers.add(0, (request, response, handler, ex) -> {
            if (response.getStatus() == HttpServletResponse.SC_NOT_FOUND) {
                try {
                    response.sendRedirect("/");
                    return new ModelAndView(); // 빈 뷰 리턴으로 끝냄
                } catch (IOException e) {
                    // 예외 무시
                }
            }
            return null; // 다른 resolver로 넘김
        });
    }
}
```


3. 또는 `web.xml`을 사용하는 경우 (Spring Boot 이전 방식)


```xml
<error-page>
    <error-code>404</error-code>
    <location>/</location>
</error-page>
<error-page>
    <error-code>500</error-code>
    <location>/</location>
</error-page>
```

Spring Boot 사용하는 경우 (application.properties)

```properties
server.error.whitelabel.enabled=false
server.error.path=/error
```

그리고 `/error` 요청을 처리하는 컨트롤러에서:

```java
@Controller
public class ErrorController implements org.springframework.boot.web.servlet.error.ErrorController {

    @RequestMapping("/error")
    public String handleError(HttpServletRequest request) {
        Object statusCode = request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);

        if (statusCode != null) {
            int status = Integer.parseInt(statusCode.toString());
            if (status == 404 || status == 500) {
                return "redirect:/";
            }
        }
        return "redirect:/";
    }
}
```

