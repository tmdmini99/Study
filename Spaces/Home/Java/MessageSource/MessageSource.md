### **`MessageSource`란?**

`MessageSource`는 **Spring의 국제화(i18n, internationalization)를 지원하는 인터페이스**로, 애플리케이션에서 다국어 메시지를 관리하고 로드하는 역할을 합니다.

Spring에서 `MessageSource`를 사용하면, `messages.properties`와 같은 파일을 기반으로 **다양한 언어의 메시지를 손쉽게 가져올 수 있습니다.**  
이 인터페이스는 **다국어 처리, 예외 메시지 처리, 사용자 UI 메시지 관리** 등에 자주 사용됩니다.

---

## **1. `MessageSource`의 주요 메서드**


```java
String getMessage(String code, Object[] args, String defaultMessage, Locale locale);
String getMessage(String code, Object[] args, Locale locale) throws NoSuchMessageException;
String getMessage(MessageSourceResolvable resolvable, Locale locale);
```


|메서드|설명|
|---|---|
|`getMessage(String code, Object[] args, String defaultMessage, Locale locale)`|코드(`code`)에 해당하는 메시지를 가져옴. 메시지가 없을 경우 기본 메시지(`defaultMessage`)를 반환|
|`getMessage(String code, Object[] args, Locale locale)`|코드(`code`)에 해당하는 메시지를 가져오지만, 존재하지 않으면 예외(`NoSuchMessageException`) 발생|
|`getMessage(MessageSourceResolvable resolvable, Locale locale)`|`MessageSourceResolvable` 객체를 기반으로 메시지를 가져옴|

---

## **2. `MessageSource` 설정 및 사용 방법**


### **(1) `ResourceBundleMessageSource`를 사용한 설정**

Spring에서는 기본적으로 `ResourceBundleMessageSource`를 이용해 **프로퍼티 파일(`.properties`)에서 메시지를 로드**합니다.

#### **✅ `application.properties` 또는 `messages.properties` 파일 생성**

`src/main/resources/messages.properties`


```ini
order.productCd=상품 코드
order.price=가격
order.notfound=주문 정보를 찾을 수 없습니다.
```


src/main/resources/messages_en.properties

```ini
order.productCd=Product Code
order.price=Price
order.notfound=Order information not found.
```

✅ `MessageSource` Bean 등록 (`@Configuration`)

```java
import org.springframework.context.MessageSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.support.ResourceBundleMessageSource;

@Configuration
public class MessageConfig {

    @Bean
    public MessageSource messageSource() {
        ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
        messageSource.setBasename("messages"); // messages.properties에서 로드
        messageSource.setDefaultEncoding("UTF-8"); // UTF-8 인코딩 설정
        return messageSource;
    }
}
```


### **(2) `MessageSource`를 서비스에서 사용하기**

#### **✅ `MessageSource`를 `@Autowired`로 주입받기**


```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.stereotype.Service;
import org.springframework.context.i18n.LocaleContextHolder;

import java.util.Locale;

@Service
public class MessageService {

    @Autowired
    private MessageSource messageSource;

    public String getMessage(String code) {
        // 현재 로케일에 맞는 메시지 반환
        return messageSource.getMessage(code, null, LocaleContextHolder.getLocale());
    }

    public String getMessage(String code, Object[] args) {
        // 메시지 코드 + 인자 포함
        return messageSource.getMessage(code, args, LocaleContextHolder.getLocale());
    }

    public String getMessageWithDefault(String code, String defaultMessage) {
        // 메시지가 없을 경우 기본 메시지 반환
        return messageSource.getMessage(code, null, defaultMessage, LocaleContextHolder.getLocale());
    }
}
```


(3) `Main` 클래스에서 사용하기


```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import java.util.Locale;

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(MessageConfig.class);
        MessageService messageService = context.getBean(MessageService.class);

        // 현재 Locale 기반 메시지 가져오기
        System.out.println(messageService.getMessage("order.productCd")); // "상품 코드" 또는 "Product Code"

        // 특정 인자를 포함하는 메시지 사용
        System.out.println(messageService.getMessage("order.price", new Object[]{"₩10,000"})); // "₩10,000"

        // 기본 메시지 설정
        System.out.println(messageService.getMessageWithDefault("order.unknown", "기본 메시지")); // "기본 메시지"
    }
}
```


