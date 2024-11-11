

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
        <value>country.countryCode</value>
        <value>/WEB-INF/properties/data2</value>  
    </list>  
</property>  
</bean>
```






만약 src/main/resources를 사용한다면 
```xml
<value>country.countryCode</value> <!-- 반드시 classpath:를 포함 -->
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


```java
@Controller
public class OrderController {

    @Autowired
    private MessageSource messageSource;

    @GetMapping("/order/detail")
    public String orderDetail(Model model) {
        // 로케일 설정 없이 메시지를 가져옵니다.
        String phoneMessage = messageSource.getMessage("countryPhone.au", null, Locale.getDefault());
        String countryCodeMessage = messageSource.getMessage("countryCode.au", null, Locale.getDefault());

        model.addAttribute("phoneMessage", phoneMessage);
        model.addAttribute("countryCodeMessage", countryCodeMessage);

        return "orderDetail";
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

여기에 들어가는 basename은
```jsp
<fmt:setBundle basename="country.isoCode" />
```

내가 선언한 property의  value와 같아야 한다
```xml
<bean id="isoSource" class="org.springframework.context.support.ResourceBundleMessageSource">  
    <property name="basename" value="country.isoCode" />  
    <property name="defaultEncoding" value="UTF-8" />  
</bean>
```





xml 설정에서 

```xml
<bean id='messageSourceAccessor' class='org.springframework.context.support.MessageSourceAccessor'>  
    <constructor-arg ref='messageSource'/>  
</bean>
```

### `MessageSourceAccessor`의 역할

`MessageSourceAccessor`는 Spring의 `MessageSource`에서 제공하는 메시지를 접근하는 편리한 방법을 제공합니다. 이 클래스는 여러 가지 메시지를 키를 통해 쉽게 가져올 수 있도록 해주며, 주로 다음과 같은 경우에 사용됩니다:

- **메시지 키에 대한 간단한 접근**: `MessageSource`를 직접 사용하는 대신, `MessageSourceAccessor`를 사용하면 메시지를 더 간단하게 가져올 수 있습니다.
- **다국어 지원**: 여러 언어에 대한 메시지를 로드하고, 필요한 언어에 따라 메시지를 선택할 수 있습니다.
- **코드 간결성**: 메시지 접근을 위해 별도로 `MessageSource`를 호출할 필요가 없으므로 코드가 간결해집니다.

### 2. 필요성 여부

- **없어도 되는 경우**: 만약 애플리케이션에서 국제화 기능을 사용하지 않거나, 메시지를 직접 `MessageSource`에서 호출해서 사용할 수 있다면 이 설정은 필요하지 않습니다.
- **사용하는 경우**: 국제화(i18n)를 적극적으로 사용하고, 여러 군데에서 메시지를 가져올 필요가 있다면 이 설정을 추가하는 것이 좋습니다.
\
필요에 따라 사용



## 프로퍼티 적용후  한글이 안나올때


root-context.xml
```xml
<!-- MessageSource 설정 -->  
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">  
    <property name="basename" value="country.countryCode" /> <!-- 패키지 경로 포함 -->  
    <property name="defaultEncoding" value="UTF-8" />  
</bean>
```
여기서 

```xml
<property name="defaultEncoding" value="UTF-8" />  
```

추가


servlet-context.xml

```xml
<!-- LocaleResolver 설정 -->  
<beans:bean id="localeResolver" class="org.springframework.web.servlet.i18n.SessionLocaleResolver">  
    <beans:property name="defaultLocale" value="ko_KR" /> <!-- 기본 Locale을 한국어로 설정 -->  
</beans:bean>  
  
<!-- LocaleChangeInterceptor 설정 -->  
<beans:bean id="localeChangeInterceptor" class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">  
    <beans:property name="paramName" value="lang" /> <!-- URL에서 lang 파라미터를 통해 Locale 변경 -->  
</beans:bean>
```



인터셉터에서 추가 
```xml
<interceptors>  
    <interceptor>  
        <mapping path="/**"/> <!-- 모든 요청에 대해 적용 -->  
        <beans:ref bean="localeChangeInterceptor"/>  
    </interceptor>  
</interceptors>
```


그래도 안될시 


![[Pasted image 20241015132634.png]]


여기 부분이 UTF-8인지 확인 
만약 아닐 시


`Editor` > `File Encodings`에서 `Project Encoding`과 `Default encoding for properties files`를 `UTF-8`로 설정


![[Pasted image 20241015132802.png]]


여기 부분들 확인 