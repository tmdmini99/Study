### **CSRF란?**

CSRF는 **Cross Site Request Forgery(사이트 간 요청 위조)**의 줄임말로 웹 취약점 중 하나입니다.

공격자가 희생자의 권한을 도용하여 특정 웹 사이트의 기능을 실행하게 할 수 있으며 이는 희생자의 의도와는 무관하게 이루어집니다.

CSRF 취약점을 이용하면 공격자가 희생자의 계정으로 네이버 카페나 인스타그램, 페이스북 등 다수의 방문자가 있는 사이트에 광고성 혹은 유해한 게시글을 업로드 하는 것도 가능해집니다.



CSRF를 통해 공격을 하기 위해선 **아래의 조건**이 만족되어야 합니다.

​

· 희생자가 공격자가 만든 피싱 사이트에 접속할 것

· 희생자가 위조 요청을 보낼 사이트(상기한 페이스북, 인스타그램 등)에 로그인 되어 있을 것

​

(단, 굳이 피싱 사이트에 접속하지 않아도 XSS에 성공한 사이트에서도 CSRF 공격을 실행할 수 있습니다.)

이와 같은 조건을 만족해야 하는 이유는 **CSRF가 서버에 직접적으로 공격을 가하는 취약점이 아니기 때문입니다.**

언뜻 보기에는 '저 조건을 다 만족할 일이 있을까?' 싶지만 자주 방문하는 사이트는 대부분 자동 로그인 기능을 사용한다는 것을 생각하면 주의가 필요합니다. 물론 네이버나 페이스북같은 사이트는 대응책이 구비되어있겠지만 보안이 허술해보이는 소규모 커뮤니티 사이트나 쇼핑몰 등에서는 자동 로그인 기능이 있어도 사용하지 않는것이 좋겠죠

![[CSRF1.png]]


```html
<img src="http://examplesite/user/logout" width="0" height="0">
```

위에서는 광고 글 작성만 예시로 들었지만 이렇게 **유저를 강제로 로그아웃 시키는 것도 가능**합니다.

(img 태그는 GET요청을 사용하기 때문에 이와 같은 코드가 가능합니다.)

공격자가 피싱 사이트에 이런 코드를 삽입해 놓았다면 피싱 사이트에 접속하는 순간 피해자는 자기 의지와는 관계없이 로그인 해 두었던 사이트에서 로그아웃 되버립니다.

실제로 2008년 옥션에서 발생했던 관리자 계정 탈취 사건도

관리자가 admin 권한으로 로그인 한 상태에서 피싱 메일을 열람하였고 피싱 메일안에 숨겨진 CSRF코드가 실행되어 관리자 권한을 탈취하게 된 것입니다.

### **CSRF 대응방법**

CSRF에 대응하는 방법은 크게 3가지가 있습니다.

​

**1. CAPTCHA 사용**

![[CSRF2.jpg]]



인터넷을 이용하는 분이라면 한번씩은 꼭 만나보셨을 CAPTCHA입니다.

**이미지를 보여주고 그 이미지에 해당하는 문자/숫자/그림이 아니라면 요청을 거부**하기 때문에 CSRF 공격을 효과적으로 방어할 수 있는 방법입니다. 귀찮긴 해도 좋은 녀석이었네요!

​

**2. Referrer 검증법**

요청이 들어올 때 request의 header에 담겨있는 referrer 값을 확인하여 같은 도메인에서 보낸 요청인지 검증하여 차단하는 방법입니다. 거의 대부분의 경우는 이 referrer 검증법으로 공격을 방어할 수 있습니다.

하지만 동일 사이트 내에서 XSS 취약점이 발견된다면 이를 통하여 CSRF 공격을 실행할 수 있다는 점을 유의해야 하며 이는 페이지 단위까지 쪼개어서 도메인 검증을 하는것으로 페이지 간 CSRF 공격을 방어할 수 있습니다.

**​**

**3. CSRF Token 사용**

사용자의 세션에 임의의 값을 저장하여 모든 요청마다 그 값을 포함하여 전송합니다.

그리고 **요청이 들어올 때 마다 백엔드에서 세션에 저장된 값과 요청으로 전송된 값이 일치하는지 검증하여 방어**하는 방법입니다. referrer 검증법과 같이 XSS를 통한 CSRF 공격에 취약하다는 특징이 있습니다.


```html
<input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>
```

먼저 스프링 시큐리티에서는 CSRF 보호 기능이 기본적으로 활성화되어 있다는 점을 기억해야 합니다. 따라서 별도로 `<security:csrf/>` 태그를 추가하지 않아도, 스프링 시큐리티는 CSRF 토큰을 생성하고 검증하는 역할을 수행합니다 [[3]](https://assu10.github.io/dev/2023/12/17/springsecurity-csrf/). 이는 `<security:http>` 태그 내에 `auto-config="true"`가 설정되어 있으며, 이로 인해 `securityNamespaceHandler`에 의해 자동으로 필요한 보안 설정들이 적용된다는 것을 의미할 수 있습니다.







java를 통해서 하는방법
```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf().disable() // CSRF 보호 활성화
            .authorizeRequests()
            .anyRequest().authenticated();
    }
}
```




pring Security XML 설정에서 특정 URL에 대해서만 **CSRF 비활성화**를 하려면, XML 단독으로는 지원되지 않습니다. 대신, **Java 설정**과 XML 설정을 함께 사용해야 합니다. 아래는 방법을 설명합니다.

---

### 방법: Java 설정으로 특정 URL에서 CSRF 비활성화

#### 1. **Java 설정 추가**

Spring Security XML 설정만 사용 중이어도, 특정 URL에서만 CSRF를 비활성화하려면 Java 설정을 추가해야 합니다:

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .csrf()
            .ignoringAntMatchers("/specific-url/**") // 특정 URL에서 CSRF 비활성화
            .and()
            .authorizeRequests()
            .anyRequest().authenticated();
    }
}

```




이 설정에서 `/specific-url/**` 패턴에 해당하는 요청은 CSRF 검사를 건너뜁니다.

---

#### 2. **XML과 Java 설정 병합**

XML 설정과 Java 설정을 병합해서 사용할 수 있습니다. 기존 XML 설정을 유지하고, 추가적으로 Java 설정에서 CSRF 예외를 설정합니다.

---

### XML 단독으로 처리하는 방법은 없다?

Spring Security XML에서는 특정 URL에 대해 CSRF를 부분적으로 비활성화하는 기능이 **직접적으로 지원되지 않습니다**. 전체 CSRF를 비활성화하거나(`disabled="true"`), 특정 URL에서 보안을 완전히 비활성화하는(`security="none"`) 방식만 가능합니다.



`GET` 방식으로 **폼 제출**을 사용하는 것은 일반적으로 문제가 없지만, **CSRF 검증**이 있는 경우 **폼을 통해 CSRF 토큰을 보내는 방법**에 따라 문제가 발생할 수 있습니다. 여기에서 주요한 문제는 **GET 방식**과 **CSRF 검증**이 어떻게 작동하는지에 관한 차이입니다.

### 1. **GET 방식과 CSRF 검증**

- **GET 요청**은 서버에서 데이터를 요청하는 방식입니다. 일반적으로 **CSRF 공격**을 방지하기 위한 방식은 **상태 변경**을 일으킬 수 있는 요청 (예: POST, PUT, DELETE)에 적용됩니다.
- **CSRF** 검증이 적용된 서버에서는 일반적으로 **POST 요청에 대해서**만 CSRF 토큰을 검증합니다.
- **GET 방식**의 요청은 **상태 변경을 일으키지 않기 때문에** CSRF 검증이 적용되지 않도록 설정할 수도 있습니다.

### 2. **왜 `GET` 방식이 문제가 될 수 있나?**

- **폼 제출 방식**에서는 주로 **CSRF 토큰을 `POST` 방식의 요청에 첨부**하는 형태로 사용됩니다. GET 요청은 상태 변경을 일으키지 않는 조회 요청이기 때문에 일반적으로 CSRF 검증이 필요하지 않도록 설계될 수 있습니다.
- 하지만 **폼을 통한 `GET` 요청**에서 **CSRF 토큰을 전달**하려는 시도가 실패하는 경우가 있을 수 있습니다. 이는 주로 서버에서 **`GET` 요청에 대한 CSRF 토큰 검증**을 **비활성화하지 않아서** 발생할 수 있습니다.

### 3. **폼을 통해 `GET` 방식으로 보내는 방식이 잘 되지 않는 이유**

- 서버가 **GET 요청**에 대해 CSRF 검증을 수행하는 경우, CSRF 토큰이 **폼 데이터로 포함**되어 있어도 이를 **검증하지 않거나 거부**할 수 있습니다. 이는 **POST 요청에만 CSRF 토큰을 검사**하는 방식으로 설계되었기 때문입니다.
- 브라우저가 **`GET` 요청**을 보낼 때, CSRF 토큰을 **헤더에 포함**하지 않고 **폼 데이터**로만 전달하게 되면 **서버에서 이를 검증하지 않거나 무시**할 수 있습니다.

### 4. **서버가 `GET` 방식에 CSRF 검증을 적용하는 경우**

만약 서버에서 **GET 방식에 대해 CSRF 검증을 강제**한다면, **CSRF 토큰을 헤더에 추가하는 방법** 외에 **폼 데이터로 전달하는 방법**은 정상적으로 작동하지 않을 수 있습니다. **헤더 방식**으로 CSRF 토큰을 전달하는 것이 더 안전한 방법입니다.

### 5. **해결 방법**

- **서버 설정 확인**: 서버가 `GET` 방식에 대해 CSRF 검증을 수행하는지 확인하세요. 만약 그렇다면, **CSRF 검증을 GET 방식에서는 제외**하도록 설정할 수 있습니다.
    - 예를 들어, **Spring Security**에서는 `csrf().ignoringAntMatchers("/your-get-url/**")`와 같은 방법으로 특정 `GET` 요청에서 CSRF 검증을 생략할 수 있습니다.
- **`GET` 요청에 CSRF 토큰을 헤더로 추가**하여 AJAX를 사용하는 방법도 고려할 수 있습니다. 이를 통해 **폼 데이터 대신 CSRF 토큰을 헤더에 포함**시킬 수 있습니다.


```js
$.ajax({
    url: targetUrl,
    method: 'GET',
    headers: {
        'X-CSRF-TOKEN': $('#csrf').val() // CSRF 토큰을 헤더에 추가
    },
    success: function(response) {
        // 성공 시 처리
    },
    error: function(xhr, status, error) {
        // 실패 시 처리
    }
});
```


`<security:http pattern="/barcodes/post/**" security="none"/>`는 **Spring Security**의 설정입니다. 이 설정은 **특정 URL 패턴에 대해 보안 설정을 비활성화**하는 역할을 합니다. 설명을 드리자면:

### 1. **`<security:http>`**: Spring Security에서 HTTP 요청에 대한 보안 설정을 정의하는 요소입니다.

- `http` 요소는 **HTTP 요청에 대한 보안 정책**을 설정하는 데 사용됩니다. 여기에는 인증, 권한 부여, CSRF 보호, CORS 설정 등 여러 가지 보안 관련 설정이 포함될 수 있습니다.

### 2. **`pattern="/barcodes/post/**"`**: 특정 URL 패턴을 지정합니다.

- 이 부분은 **Spring Security**가 보안 정책을 적용할 URL 패턴을 지정하는 곳입니다.
- `"/barcodes/post/**"`는 **`/barcodes/post/`로 시작하는 모든 경로**에 대해 보안 설정을 적용하겠다는 의미입니다. 예를 들어, `/barcodes/post/something`이나 `/barcodes/post/anything` 등이 포함됩니다.

### 3. **`security="none"`**: 보안을 적용하지 않음.

- `security="none"`는 **해당 경로에 대한 보안 처리를 **아예 비활성화**하겠다는 의미입니다.
- 즉, **인증(authentication), 권한 부여(authorization), CSRF 보호 등** 어떤 보안 검사를 하지 않겠다는 설정입니다. 이 설정을 사용하면 해당 경로에 대한 모든 보안 처리가 우회됩니다.

### 4. **전체적인 의미**

- `"/barcodes/post/**"` 경로로 들어오는 요청에 대해서 **Spring Security의 기본 보안 설정을 적용하지 않겠다**는 의미입니다.
    - 예를 들어, 해당 URL에서 로그인 상태나 권한을 확인하지 않게 됩니다.
    - CSRF 보호, 인증, 권한 검사를 **하지 않도록 설정**하게 됩니다.

```xml
<security:http pattern="/barcodes/post/**" security="none"/>
```






---
참조 - https://tibetsandfox.tistory.com/11
