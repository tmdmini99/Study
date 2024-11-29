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


---
참조 - https://tibetsandfox.tistory.com/11
