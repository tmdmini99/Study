

설정

단일 설정
```xml
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">  
    <property name="basename" value="country.countryCode" /> <!-- 패키지 경로 포함 -->  
</bean>
```

properties 여러개 설정

```xml
<bean class="org.springframework.context.support.ReloadableResourceBundleMessageSource" id="messageSource">  
  
<property name="basenames">  <!-- 여러개인 경우 -->  
    <list>  
        <value>classpath:country.countryCode</value>
        <value>/WEB-INF/properties/data2</value>  
    </list>  
</property>  
</bean>
```
반드시 classpath를 포함시킬




만약 src/main/resources를 사용한다면 
```xml
<value>classpath:country.countryCode</value> <!-- 반드시 classpath:를 포함 -->
```
이런식으로 country.countryCode로 사용해야 하고


만약 webapp 밑에 resources 를 사용한다면 
```xml
<value>/resources/css/common.css</value>
```
이런식으로 사용해야 함


properties 값
```properties
greeting.message=안녕하세요!
farewell.message=안녕히 가세요!
```

설정한 properties 가져오는 법 java

```java
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class GreetingController {

    @Value("${greeting.message}") // properties 파일에서 greeting.message 값을 가져옵니다.
    private String greetingMessage;

    @GetMapping("/greeting")
    public String greeting(Model model) {
        model.addAttribute("greetingMessage", greetingMessage); // 값을 모델에 추가
        return "greeting"; // greeting.jsp로 이동
    }
}

```


jsp

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>동적으로 속성 파일 값 출력</title>
</head>
<body>
    <%-- 속성 파일 로드 --%>
    <fmt:setBundle basename="messages" />
    
    <%-- 전달된 messageKey 값에 따라 동적으로 properties 파일에서 값 출력 --%>
    <p><fmt:message key="${messageKey}" /></p>
</body>
</html>

```

fmt를 사용하여 가져옴