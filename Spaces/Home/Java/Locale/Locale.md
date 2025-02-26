# **Java `Locale` 개념 및 설정 방법**

`Locale`은 특정 지역(국가, 언어, 문화권) 정보를 포함하는 **국제화(i18n) 및 지역화(l10n)** 지원을 위한 Java의 클래스입니다.  
날짜, 시간, 숫자, 통화, 메시지 번역 등을 처리할 때 사용됩니다.

---

## **1. `Locale` 클래스 개요**

Java의 `java.util.Locale` 클래스는 **언어(Language), 국가(Country), 지역(Variant)** 정보를 저장하고 있습니다.


```java
Locale locale = new Locale("ko", "KR"); // 한국어, 한국
```

**✔ `Locale`의 주요 정보**

- **언어(Language)**: ISO 639 언어 코드 (예: `"ko"`, `"en"`, `"fr"`)
- **국가(Country)**: ISO 3166 국가 코드 (예: `"KR"`, `"US"`, `"FR"`)
- **지역(Variant)**: 특정 지역 변형 정보 (거의 사용되지 않음)

---

## **2. `Locale` 생성 방법**

### **(1) 기본 생성자 사용**


```java
Locale locale = new Locale("ko", "KR"); // 한국어, 한국
Locale usLocale = new Locale("en", "US"); // 영어, 미국
Locale frLocale = new Locale("fr", "FR"); // 프랑스어, 프랑스
```

### **(2) `Locale.Builder` 사용 (Java 7 이상)**

가독성이 좋으며 `setLanguage()`, `setRegion()` 등을 체이닝 방식으로 설정할 수 있음.


```java
Locale locale = new Locale.Builder()
    .setLanguage("ko")
    .setRegion("KR")
    .build();
```


### **(3) `Locale`의 정적 상수 사용**

Java에서는 몇 가지 자주 사용하는 `Locale`을 **상수 값**으로 제공함.


```java
Locale locale1 = Locale.KOREA;      // 한국 (ko_KR)
Locale locale2 = Locale.US;         // 미국 (en_US)
Locale locale3 = Locale.JAPAN;      // 일본 (ja_JP)
Locale locale4 = Locale.FRANCE;     // 프랑스 (fr_FR)
```


(4) 시스템 기본 로케일 가져오기

```java
Locale defaultLocale = Locale.getDefault();
System.out.println("현재 시스템 Locale: " + defaultLocale);
```

(5) 시스템 기본 로케일 변경하기

```java
Locale.setDefault(new Locale("en", "US"));
System.out.println("변경된 시스템 Locale: " + Locale.getDefault());
```

## **3. `Locale`의 주요 메서드**

### **✔ `getLanguage()` - 언어 코드 가져오기**

```java
Locale locale = new Locale("ko", "KR");
System.out.println(locale.getLanguage()); // "ko"
```


✔ `getCountry()` - 국가 코드 가져오기

```java
System.out.println(locale.getCountry()); // "KR"
```

✔ `getDisplayLanguage()` - 언어 이름 가져오기

```java
System.out.println(locale.getDisplayLanguage()); // "Korean"
System.out.println(locale.getDisplayLanguage(Locale.ENGLISH)); // "Korean"
System.out.println(locale.getDisplayLanguage(Locale.KOREA)); // "한국어"
```

✔ `getDisplayCountry()` - 국가 이름 가져오기

```java
System.out.println(locale.getDisplayCountry()); // "South Korea"
System.out.println(locale.getDisplayCountry(Locale.KOREA)); // "대한민국"
```


✔ `getISO3Language()` - ISO 3자리 언어 코드 가져오기

```java
System.out.println(locale.getISO3Language()); // "kor"
```

✔ `getISO3Country()` - ISO 3자리 국가 코드 가져오기
```java
System.out.println(locale.getISO3Country()); // "KOR"
```


## **4. `Locale`를 활용한 다양한 설정**

### **(1) 날짜 및 시간 형식 (`DateFormat`)**


```java
import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

public class LocaleDateExample {
    public static void main(String[] args) {
        Date now = new Date();

        // 한국 로케일
        DateFormat dfKr = DateFormat.getDateInstance(DateFormat.LONG, Locale.KOREA);
        System.out.println("한국 날짜 형식: " + dfKr.format(now));

        // 미국 로케일
        DateFormat dfUs = DateFormat.getDateInstance(DateFormat.LONG, Locale.US);
        System.out.println("미국 날짜 형식: " + dfUs.format(now));

        // 프랑스 로케일
        DateFormat dfFr = DateFormat.getDateInstance(DateFormat.LONG, Locale.FRANCE);
        System.out.println("프랑스 날짜 형식: " + dfFr.format(now));
    }
}
```


📌 출력 예시:

```bash
한국 날짜 형식: 2025년 2월 26일
미국 날짜 형식: February 26, 2025
프랑스 날짜 형식: 26 février 2025
```


(2) 숫자 및 통화 형식 (`NumberFormat`)

```java
import java.text.NumberFormat;
import java.util.Locale;

public class LocaleNumberExample {
    public static void main(String[] args) {
        double price = 1234567.89;

        NumberFormat nfKr = NumberFormat.getCurrencyInstance(Locale.KOREA);
        NumberFormat nfUs = NumberFormat.getCurrencyInstance(Locale.US);
        NumberFormat nfFr = NumberFormat.getCurrencyInstance(Locale.FRANCE);

        System.out.println("한국 통화 형식: " + nfKr.format(price)); // ₩1,234,568
        System.out.println("미국 통화 형식: " + nfUs.format(price)); // $1,234,567.89
        System.out.println("프랑스 통화 형식: " + nfFr.format(price)); // 1 234 567,89 €
    }
}
```



### **(3) 다국어 메시지 처리 (`MessageSource`)**

위에서 설명한 `MessageSource`와 함께 `Locale`을 적용하면 다국어 지원이 가능함.


```java
import org.springframework.context.MessageSource;
import org.springframework.context.support.ResourceBundleMessageSource;
import java.util.Locale;

public class LocaleMessageExample {
    public static void main(String[] args) {
        MessageSource messageSource = new ResourceBundleMessageSource();
        ((ResourceBundleMessageSource) messageSource).setBasename("messages");
        ((ResourceBundleMessageSource) messageSource).setDefaultEncoding("UTF-8");

        System.out.println(messageSource.getMessage("order.productCd", null, new Locale("ko", "KR"))); // "상품 코드"
        System.out.println(messageSource.getMessage("order.productCd", null, new Locale("en", "US"))); // "Product Code"
    }
}
```


## **5. `Locale`과 웹 애플리케이션에서의 활용**

Spring MVC에서는 `LocaleResolver`를 통해 클라이언트의 언어를 자동 감지하고 적용할 수 있음.

### **✅ `LocaleResolver`를 이용한 동적 로케일 변경**


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.SessionLocaleResolver;
import java.util.Locale;

@Configuration
public class LocaleConfig {
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver resolver = new SessionLocaleResolver();
        resolver.setDefaultLocale(Locale.KOREA); // 기본 로케일 설정
        return resolver;
    }
}
```


✅ 클라이언트 요청으로 로케일 변경 (`?lang=en`)


```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.LocaleResolver;
import javax.servlet.http.HttpServletRequest;
import java.util.Locale;

@RestController
public class LocaleController {
    private final LocaleResolver localeResolver;

    public LocaleController(LocaleResolver localeResolver) {
        this.localeResolver = localeResolver;
    }

    @GetMapping("/change-locale")
    public String changeLocale(@RequestParam String lang, HttpServletRequest request) {
        localeResolver.setLocale(request, null, new Locale(lang));
        return "Locale changed to: " + lang;
    }
}
```

## **결론**

- `Locale`은 **언어, 국가 정보를 관리하는 Java의 표준 클래스**
- `DateFormat`, `NumberFormat`, `MessageSource`와 함께 사용하면 **국제화(i18n) 지원 가능**
- Spring 환경에서는 `LocaleContextHolder.getLocale()`과 `LocaleResolver`를 사용하여 **자동 감지 및 변경 가능**
- 다국어 지원을 위해 `MessageSource`와 함께 활용 가능



# **`LocaleResolver`란?**

`LocaleResolver`는 **Spring MVC에서 클라이언트의 로케일(Locale)을 결정하고 설정하는 인터페이스**입니다.  
즉, 사용자의 **언어 및 지역(Locale)을 감지하고, 해당 정보를 기반으로 다국어(i18n) 지원을 가능하게 해줍니다.**

📌 **주요 역할**

- 클라이언트의 **요청(Request)에서 로케일을 감지**
- 감지된 로케일을 기반으로 **메시지 번역, 날짜/숫자 형식 변경** 등을 수행
- **세션, 쿠키, HTTP 헤더** 등을 활용하여 로케일을 저장하고 관리


## **1. `LocaleResolver`의 동작 방식**

Spring MVC에서 `LocaleResolver`를 사용하면 다음과 같은 흐름으로 동작합니다.

### **✔ `LocaleResolver` 동작 순서**

1. **클라이언트의 언어 요청 감지**
    
    - 요청 헤더(`Accept-Language`), 세션, 쿠키, URL 파라미터 등에서 `Locale` 값을 가져옴.
2. **로케일 설정**
    
    - 감지한 `Locale`을 Spring의 `LocaleContextHolder`에 저장하여 전역적으로 사용 가능.
3. **국제화(i18n) 메시지 적용**
    
    - `MessageSource`와 연결되어 적절한 번역된 텍스트를 제공.

---

## **2. `LocaleResolver`의 종류**

Spring에서는 다양한 `LocaleResolver`를 제공하며, 사용 목적에 맞게 선택할 수 있습니다.

|**타입**|**설명**|
|---|---|
|`AcceptHeaderLocaleResolver` (기본)|HTTP 요청 헤더(`Accept-Language`)를 기반으로 로케일을 감지|
|`SessionLocaleResolver`|세션(Session)에 로케일을 저장하여 유지|
|`CookieLocaleResolver`|쿠키를 사용하여 로케일을 저장 및 유지|
|`FixedLocaleResolver`|특정 로케일을 **고정**하여 사용 (변경 불가)|

---

## **3. `LocaleResolver` 설정 및 사용법**

### **✅ (1) `SessionLocaleResolver` 설정 (세션 기반 로케일)**

> **세션에 로케일을 저장하여 유지하는 방식**
> 
> - 사용자가 언어를 변경하면 **세션이 유지되는 동안 동일한 언어가 적용됨.**

#### **🔹 `LocaleResolver` 설정**


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.SessionLocaleResolver;

import java.util.Locale;

@Configuration
public class LocaleConfig {
    @Bean
    public LocaleResolver localeResolver() {
        SessionLocaleResolver resolver = new SessionLocaleResolver();
        resolver.setDefaultLocale(Locale.KOREA); // 기본 로케일을 한국어로 설정
        return resolver;
    }
}
```


🔹 컨트롤러에서 로케일 변경 API 추가


```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.LocaleResolver;

import javax.servlet.http.HttpServletRequest;
import java.util.Locale;

@RestController
public class LocaleController {
    private final LocaleResolver localeResolver;

    public LocaleController(LocaleResolver localeResolver) {
        this.localeResolver = localeResolver;
    }

    @GetMapping("/change-locale")
    public String changeLocale(@RequestParam String lang, HttpServletRequest request) {
        Locale newLocale = new Locale(lang);
        localeResolver.setLocale(request, null, newLocale);
        return "Locale changed to: " + newLocale.getLanguage();
    }
}
```


📌 **테스트 방법**  
웹 브라우저에서 다음과 같이 요청하면 **로케일이 변경됨**


```bash
http://localhost:8080/change-locale?lang=en  (영어로 변경)
http://localhost:8080/change-locale?lang=ko  (한국어로 변경)
```


### **✅ (2) `CookieLocaleResolver` 설정 (쿠키 기반 로케일)**

> **쿠키를 사용하여 로케일을 저장하는 방식**
> 
> - 사용자가 브라우저를 닫아도 쿠키가 남아 있어 **다음 방문 시에도 동일한 로케일을 유지**할 수 있음.

#### **🔹 `LocaleResolver` 설정**


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.CookieLocaleResolver;

import java.util.Locale;

@Configuration
public class LocaleConfig {
    @Bean
    public LocaleResolver localeResolver() {
        CookieLocaleResolver resolver = new CookieLocaleResolver();
        resolver.setDefaultLocale(Locale.KOREA); // 기본 로케일 한국어
        resolver.setCookieName("lang"); // 쿠키 이름 설정
        resolver.setCookieMaxAge(3600); // 1시간 동안 유지
        return resolver;
    }
}
```



✔ 이 설정을 적용하면, 클라이언트가 로케일을 변경하면 **쿠키에 저장**되며 이후에도 유지됨.

---

### **✅ (3) `AcceptHeaderLocaleResolver` 설정 (기본값, HTTP 헤더 기반)**

> **클라이언트의 `Accept-Language` 헤더를 기반으로 로케일을 자동 감지**
> 
> - 가장 일반적인 방식이며, 별도의 설정이 없어도 동작함.
> - 하지만 **사용자가 직접 로케일을 변경할 수 없음.**

#### **🔹 기본 사용 (설정 필요 없음)**


```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver;

import java.util.Locale;

@Configuration
public class LocaleConfig extends AcceptHeaderLocaleResolver {
    public LocaleConfig() {
        this.setDefaultLocale(Locale.KOREA); // 기본 로케일을 한국어로 설정
    }
}
```


✔ `AcceptHeaderLocaleResolver`는 **별도로 로케일을 변경하는 API가 필요 없음**  
✔ 클라이언트가 브라우저 설정을 변경하면 **자동으로 해당 언어를 감지하여 적용**

---

### **✅ (4) `FixedLocaleResolver` 설정 (고정 로케일)**

> **어떤 경우에도 로케일을 변경하지 않고 특정 언어만 강제 적용**
> 
> - 국제화가 필요 없고 단일 언어만 사용하는 경우 유용함.

#### **🔹 설정 방법**


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.FixedLocaleResolver;

import java.util.Locale;

@Configuration
public class LocaleConfig {
    @Bean
    public LocaleResolver localeResolver() {
        return new FixedLocaleResolver(Locale.KOREA); // 항상 한국어로 고정
    }
}
```


✔ 어떤 요청이든 **항상 `ko_KR` 로케일이 적용됨.**  
✔ `MessageSource`, `NumberFormat`, `DateFormat` 등이 **한국어 설정으로만 동작**.

---

## **4. `LocaleResolver`와 `MessageSource` 연동**

위에서 설정한 `LocaleResolver`와 함께 `MessageSource`를 사용하면 다국어 지원이 가능함.

#### **✅ `MessageSource` 설정**


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
        messageSource.setBasename("messages"); // messages.properties 사용
        messageSource.setDefaultEncoding("UTF-8");
        return messageSource;
    }
}
```


📌 **국제화 파일 (`messages.properties`)**


```java
messages.properties (기본)
order.success=주문이 완료되었습니다.

messages_en.properties (영어)
order.success=Your order has been placed.
```


이제 컨트롤러에서 메시지를 조회하면, `LocaleResolver`가 설정한 로케일에 맞춰 **자동으로 번역된 메시지가 반환**됨.

---

## **5. 결론**

- `LocaleResolver`는 **Spring MVC에서 사용자의 로케일을 결정하는 역할**
- **주요 구현체**
    - `SessionLocaleResolver` → **세션 기반** (서버 내에서 유지)
    - `CookieLocaleResolver` → **쿠키 기반** (브라우저 종료 후에도 유지)
    - `AcceptHeaderLocaleResolver` → **HTTP 헤더 기반** (자동 감지)
    - `FixedLocaleResolver` → **변경 불가능한 고정 로케일**
- `MessageSource`와 결합하면 **다국어 메시지를 자동으로 번역** 가능.