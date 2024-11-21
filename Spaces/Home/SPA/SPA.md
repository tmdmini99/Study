### 짧은 요약

- SPA는 하나의 페이지로 이루어진 웹이고 MPA는 여러 페이지로 이루어진 웹이다.
- 초기 로딩은 MPA가 빠르고 그 후의 페이지 전환은 SPA가 빠르다.
- SEO에는 'SSR 방식으로 동작하는' MPA가 더 유리하다.


# SPA의 등장 배경 - MPA

전통적으로 웹 브라우저는 서버를 호출할 때마다 새 HTML 페이지를 응답받습니다. 즉 페이지가 여러개라는 뜻이죠. 페이지가 여러개니까 여러 HTML 파일이 있고 새로운 페이지를 서버에 요청(request)할 때마다 정적 리소스가 다운로드 되고(response), 그에 맞춰 `다시 전체 페이지를 렌더링`하는 방식입니다.

이러한 전통적인 방식은 여러 개의(Multiple) 페이지로(Page) 이루어진 웹앱(Application)이라고 해서 `MPA` (Multiple Page Application)방식이라고 불렸습니다.

> #### 정적 리소스란 무엇인가요?
> 
> 정적 리소스는 서버 컴퓨터 내에 저장되어 있는 HTML, CSS, JavaScript, 이미지, 영상 등을 말합니다. 클라이언트와 서버간 상호작용을 통해 정보 및 자료가 생성되는 게 아니라, 단순히 `서버에 존재하는 파일`이기 때문에 ‘정적' 리소스라는 용어를 사용합니다.

---

## MPA의 단점

근데 페이지가 바뀔 때마다 매번 완전한 페이지를 응답받아야 해서 필요한 부분만 응답받는 방식에 비해 효율이 떨어집니다. 페이지 이동 시 `불필요한 로딩`을 다시 하기 때문에 서버 렌더링에 따른 부하가 존재하는 것이죠. 즉 성능상에 문제가 있는 것입니다.

마찬가지로 전체 페이지를 다시 로딩하니까 다른 페이지로 넘어가거나 새로고침을 하면 화면이 깜빡입니다. 이는 곧 `안 좋은 사용자 경험`을 초래하게 됩니다.

페이지 규모가 작으면 괜찮지만, 데스크탑 브라우저에 비해 성능이 떨어지는 모바일 기기가 새롭게 등장하면서 이런 MPA 방식은 `모바일 환경에서 큰 부담`을 주게 됩니다.

---

# SPA의 등장

MPA의 성능상의 문제와 나쁜 사용자 경험을 개선하기 위해 SPA라는 새로운 방식이 등장합니다. 단일(Single) HTML 페이지(Page)를 로드하고 사용자가 앱과 상호 작용할 때 해당 페이지를 `동적`으로 업데이트하는 웹앱(Application)을 뜻한다고 해서 `SPA` (Single Page Application)방식이라고 불립니다.

SPA는 AJAX 및 HTML5를 사용하여 지속적인 페이지 새로고침 없이 유연하고 반응이 빠른 웹앱을 만들 수 있습니다.

SPA에서 처음 페이지가 로드된 후, 서버와의 모든 상호 작용은 `AJAX` 호출을 통해 이루어집니다. 이러한 AJAX 호출은 일반적으로 JSON 형식으로, 마크업이 아닌 데이터를 반환합니다. 앱은 `페이지를 다시 로드하지 않고 페이지를 동적으로 업데이트하기 위해 ✨JSON 데이터를 사용`합니다.

아래 그림은 두 접근 방식의 차이점을 보여줍니다.


![[SPA1.png]]



![[SPA2.jpg]]



![[SPA3.jpg]]









## SPA의 장점

SPA는 전체 페이지를 다시 로드하고 다시 렌더링하는 혼란스러운 과정이 없기 때문에 자연스러운 페이지 이동과 사용자 경험을 제공합니다. 즉 `성능`과 `사용자 경험 (UX)` 두 마리 토끼를 다 잡는 셈이죠.

## SPA와 MPA 비교🧐

### 페이지 로드 속도

사용자가 처음 url에 접근하면 웹 페이지에 필요한 `모든` 정적 리소스를 `한 번에` 클라이언트로 받아옵니다. 하지만 바꿔 말하면 초기 페이지 로딩이 MPA 방식에 비해 오래 걸립니다.

그 다음부터는 페이지를 전환할 때 다시 서버로 요청을 보내는 것이 아닌, JS에 의해 `필요한 데이터`만 `동적`으로 받아옵니다. 즉 JS가 `라우팅`을 담당합니다. 따라서 초기 로딩 이후부터는 SPA방식이 MPA 방식보다 페이지 로드 속도가 빠릅니다.

> #### 라우팅은 뭔가요?
> 
> 사용자가 요청한 URL에 따른 페이지로 전환하기 위해 필요한 데이터를 서버에 요청하고 `페이지를 전환`하는 일련의 행위를 말합니다.

### SEO 관점⚙️

SPA는 페이지 전환이 빠르다는 장점이 있지만 `SEO에 불리`하다는 단점이 존재합니다. 구글 검색엔진은 SPA도 크롤링을 잘 하지만 국내 검색엔진의 경우 SEO가 어렵습니다. SPA는 화면을 띄우는데 JS가 사용되고 구글 검색엔진은 JS를 해석할 수 있지만 국내 검색엔진은 JS를 해석하지 못하기 때문이죠.

일반적인 SPA 앱을 검색 로봇 입장에서 보면 모든 페이지의 소스가 아래와 같습니다.

```html
<html>
<head>
  <title>Single Page Application</title>
  <link rel="stylesheet" href="app.css" type="text/css">
</head>
<body>
  <div id="app"></div>
  <script src="app.js"></script>
</body>
</html>
```

즉 로딩시점에서는 검색 엔진이 색인을 할 만한 콘텐츠가 존재하지 않는 것이죠.

반면에 'SSR 방식으로 동작하는' MPA는 애초에 완성된 형태의 HTML을 서버로부터 전달받기 때문에 검색 엔진이 페이지를 크롤링 하기에 적합하므로 SEO에 유리합니다.

---

### MPA == SSR, SPA == CSR이다?❌

바로 위에서 'SSR 방식으로 동작하는' MPA는 SEO에 유리하다고 언급했습니다. 'SSR 방식으로 동작하는'이 중요합니다. 그 이유는 다음과 같습니다.

흔히 MPA == SSR, SPA == CSR 이렇게 생각할 수도 있는데 그렇지 않습니다. React같은 JS 기반 프레임워크 혹은 라이브러리를 사용하면 기본적으로 하나의 HTML, CSS, JS 파일이 나오기 때문에 자연스레 SPA이면서 CSR이 되는 것이고, 이를 SSR로 구현하면 페이지 별로 렌더링을 따로 하기 때문에 MPA가 되는 것입니다.

MPA면서 같은 JS 파일을 받고 클라이언트에서 렌더링을 하게 되면 MPA + CSR이 되는 것이고 SPA면서 SSR을 택하면 SSR + SPA가 됩니다. 원리를 이해하면 이런 식으로는 궁합이 잘 맞지 않기 때문에 MPA == SSR, SPA == CSR이 마치 공식인 것 처럼 생각되기도 합니다.

따라서 CSR 방식으로 동작하는 MPA의 경우도 가능하기 때문에 MPA는 무조건 SEO에 유리하다고 해버리면 이는 틀린 말이 되는 것입니다.

Next.js와 같은 React 라이브러리의 프레임워크를 사용하면 흔히 CSR로 동작하는 SPA도 SSR 방식으로 설계가 가능하기 때문에, SEO 문제를 해결할 수 있습니다!

### **React 사용 시 SPA 구현예시**

다음은 SPA(Single Page Application)를 React를 사용하여 구성하는 간단한 예시입니다.



![[SPA4.jpg]]



###     Java Script 사용 SPA 구현예시

다음은 SPA(Single Page Application)를 순수 Java Script(Vanilla Java Script)를 사용하여 구성하는 간단한 예시입니다

![[SPA5.jpg]]





## **백엔드(Spring Legacy) 설정**

### 2.1. **RESTful API 컨트롤러 만들기**

기존 JSP 또는 Thymeleaf 기반의 페이지 렌더링을 제거하고, RESTful API를 반환하도록 컨트롤러를 수정합니다.


```java
@RestController
@RequestMapping("/api")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/users")
    public ResponseEntity<List<User>> getAllUsers() {
        List<User> users = userService.getAllUsers();
        return ResponseEntity.ok(users); // JSON 형태로 반환
    }

    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
}

```


**특징**:

- `@RestController`: JSON 데이터를 반환.
- `ResponseEntity`: HTTP 상태코드와 데이터를 명시적으로 전달.



### 2.2. **CORS(Cross-Origin Resource Sharing) 설정**

SPA는 백엔드와 프런트엔드가 서로 다른 도메인에서 동작하기 때문에 CORS 설정이 필요합니다.


```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000") // React/Vue 개발 서버 주소
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowCredentials(true);
    }
}

```



### 2.3. **정적 리소스 핸들링**

SPA 빌드 결과물(`index.html`, `bundle.js` 등)을 Spring에서 제공하려면, 정적 파일을 `src/main/resources/static`에 배치해야 합니다.

1. 빌드된 프런트엔드 파일을 복사:
    
    - React: `npm run build` 실행 후 생성된 `build/` 폴더 내용을 `src/main/resources/static`에 복사.
    - Vue.js: `npm run build` 실행 후 `dist/` 폴더 내용을 복사.
2. 메인 요청 처리:
    
    - SPA의 진입점인 `index.html`을 반환하도록 설정.


```java
@Controller
public class HomeController {
    @GetMapping("/")
    public String index() {
        return "index"; // static/index.html 반환
    }
}

```



## 3. **프런트엔드(SPA) 설정**

### 3.1. **React/Vue 프로젝트 생성**

React 또는 Vue.js와 같은 SPA 프레임워크를 선택하여 프로젝트를 생성합니다.

#### React 프로젝트 생성:


```bash
npx create-react-app my-app
cd my-app
```



Vue.js 프로젝트 생성:



```bash
npm install -g @vue/cli
vue create my-app
cd my-app
```


### 3.2. **API 호출 코드 작성**

SPA에서 백엔드 API를 호출하여 데이터를 가져오는 코드를 작성합니다.

#### React 예제:


```js
import React, { useEffect, useState } from "react";

function App() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("http://localhost:8080/api/users")
      .then(response => response.json())
      .then(data => setUsers(data));
  }, []);

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```


Vue.js 예제:


```js
<template>
  <div>
    <h1>User List</h1>
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: []
    };
  },
  mounted() {
    fetch("http://localhost:8080/api/users")
      .then(response => response.json())
      .then(data => {
        this.users = data;
      });
  }
};
</script>
```



### 3.3. **SPA 빌드 및 배포**

1. **프런트엔드 빌드**:
    
    - React:
- 

```bash
npm run build
```


- 결과물이 `build/` 폴더에 생성됩니다.
- Vue.js

```bash
npm run build
```


- - 결과물이 `dist/` 폴더에 생성됩니다.
- **Spring Legacy로 배포**:
    
    - 빌드된 파일(`build/` 또는 `dist/` 내용)을 Spring의 `src/main/resources/static`에 복사.





### SPA에서 JWT를 활용한 인증 구현

JWT(JSON Web Token)는 클라이언트와 서버 간 인증을 간단하고 안전하게 처리할 수 있는 방식으로, 특히 SPA와 같이 상태 비저장(stateless) 방식의 애플리케이션에 적합합니다.

---

## **JWT 인증의 기본 흐름**

1. **사용자 로그인**: 사용자가 아이디/비밀번호로 로그인 요청.
2. **JWT 발급**: 서버는 인증 정보를 검증하고 JWT를 생성하여 클라이언트에 반환.
3. **클라이언트 저장**: 클라이언트는 JWT를 로컬 스토리지 또는 쿠키에 저장.
4. **요청 시 토큰 전송**: 클라이언트는 이후 API 요청마다 JWT를 `Authorization` 헤더에 담아 전송.
5. **서버 검증**: 서버는 수신된 JWT의 유효성을 검증하고 요청을 처리.

---

## **Spring에서 JWT 인증 구현**

### 1. **필요한 라이브러리 추가**

Spring 프로젝트의 `pom.xml`에 JWT 관련 라이브러리를 추가합니다.


pom.xml
```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>

```


### 2. **JWT 유틸리티 클래스 작성**

JWT 생성, 검증, 파싱 등을 담당하는 유틸리티 클래스를 작성합니다.


```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;

import java.security.Key;
import java.util.Date;

public class JwtUtil {

    private static final String SECRET_KEY = "my-secret-key-for-jwt-my-secret-key"; // 32자 이상
    private static final long EXPIRATION_TIME = 1000 * 60 * 60; // 1시간

    private static final Key key = Keys.hmacShaKeyFor(SECRET_KEY.getBytes());

    public static String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date())
                .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))
                .signWith(key, SignatureAlgorithm.HS256)
                .compact();
    }

    public static Claims validateToken(String token) throws JwtException {
        return Jwts.parserBuilder()
                .setSigningKey(key)
                .build()
                .parseClaimsJws(token)
                .getBody();
    }
}
```


### 3. **JWT 발급 엔드포인트 생성**

사용자가 로그인하면 JWT를 발급하는 컨트롤러를 작성합니다.


```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest loginRequest) {
        // 사용자 인증 (여기선 간단히 하드코딩)
        if ("user".equals(loginRequest.getUsername()) && "password".equals(loginRequest.getPassword())) {
            String token = JwtUtil.generateToken(loginRequest.getUsername());
            return ResponseEntity.ok(new LoginResponse(token));
        } else {
            return ResponseEntity.status(401).body("Invalid credentials");
        }
    }

    public static class LoginRequest {
        private String username;
        private String password;

        // Getter, Setter
        public String getUsername() { return username; }
        public void setUsername(String username) { this.username = username; }
        public String getPassword() { return password; }
        public void setPassword(String password) { this.password = password; }
    }

    public static class LoginResponse {
        private String token;

        public LoginResponse(String token) {
            this.token = token;
        }

        public String getToken() {
            return token;
        }
    }
}
```


### 4. **JWT 인증 필터 추가**

JWT를 검증하는 Spring Security 필터를 작성합니다.


```java
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {
        String authHeader = request.getHeader("Authorization");
        if (authHeader != null && authHeader.startsWith("Bearer ")) {
            String token = authHeader.substring(7);
            try {
                Claims claims = JwtUtil.validateToken(token);
                // 여기서 필요한 권한이나 정보를 Context에 추가 가능
                UsernamePasswordAuthenticationToken authentication =
                        new UsernamePasswordAuthenticationToken(claims.getSubject(), null, null);
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            } catch (JwtException e) {
                response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
                response.getWriter().write("Invalid token");
                return;
            }
        }
        filterChain.doFilter(request, response);
    }
}
```


### 5. **Spring Security 설정**

JWT 필터를 Spring Security에 추가합니다.


```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                .authorizeRequests()
                .antMatchers("/api/auth/**").permitAll() // 인증 없이 접근 가능
                .anyRequest().authenticated()
                .and()
                .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```


security-context.xml


```xml
<beans:beans xmlns="http://www.springframework.org/schema/security"
             xmlns:beans="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://www.springframework.org/schema/security
                                 http://www.springframework.org/schema/security/spring-security.xsd
                                 http://www.springframework.org/schema/beans
                                 http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 비밀번호 암호화를 위한 PasswordEncoder -->
    <beans:bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

    <!-- Custom JWT Authentication Filter -->
    <beans:bean id="jwtAuthenticationFilter" class="com.example.JwtAuthenticationFilter"/>

    <!-- Spring Security Configuration -->
    <http pattern="/api/auth/**" security="none"/> <!-- 인증 없이 접근 가능 -->

    <http>
        <csrf disabled="true"/>
        <intercept-url pattern="/**" access="isAuthenticated()"/> <!-- 인증 필요 -->

        <!-- JWT 필터를 UsernamePasswordAuthenticationFilter 전에 추가 -->
        <custom-filter ref="jwtAuthenticationFilter" before="USERNAME_PASSWORD_AUTHENTICATION_FILTER"/>
    </http>

</beans:beans>
```


### 2. **핵심 변환 내용**

1. **PasswordEncoder 등록**

```java
@Bean
public PasswordEncoder passwordEncoder() {
    return new BCryptPasswordEncoder();
}
```


```xml
<beans:bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>
```


```java
@Configuration
public class JwtAuthenticationFilter extends OncePerRequestFilter { ... }
```


```xml
<beans:bean id="jwtAuthenticationFilter" class="com.example.JwtAuthenticationFilter"/>
```

- **HTTP 설정**
    
    - `csrf().disable()` → `<csrf disabled="true"/>`
    - `antMatchers("/api/auth/**").permitAll()` → `<http pattern="/api/auth/**" security="none"/>`
    - `addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class)` → `<custom-filter ref="jwtAuthenticationFilter" before="USERNAME_PASSWORD_AUTHENTICATION_FILTER"/>`
- **Authenticated URL 설정**

```java
.anyRequest().authenticated()
```


```xml
<intercept-url pattern="/**" access="isAuthenticated()"/>
```



### 3. **XML 설정 적용**

Spring Security XML을 사용하려면 `web.xml` 또는 `dispatcher-servlet.xml`에서 해당 Security 설정을 등록합니다.

#### 예: `web.xml`에 Security 설정 추가


```xml
<web-app>
    <!-- Spring Security 설정 -->
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```


예: `dispatcher-servlet.xml`에 Security 설정 참조


```xml
<import resource="classpath:security-config.xml"/>

```



### 6. **클라이언트(SPA)에서 JWT 활용**

프런트엔드에서 로그인 후 JWT를 저장하고, API 요청 시 헤더에 추가합니다.

#### 로그인 요청 (React/Vue 예시):

```js
fetch("http://localhost:8080/api/auth/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username: "user", password: "password" }),
})
    .then(res => res.json())
    .then(data => {
        localStorage.setItem("token", data.token); // JWT 저장
    });
```


API 요청:

```js
fetch("http://localhost:8080/api/users", {
    headers: {
        Authorization: `Bearer ${localStorage.getItem("token")}`,
    },
})
    .then(res => res.json())
    .then(data => console.log(data));
```









---
출처 - https://velog.io/@dikum98/SPA-Single-Page-Application%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94

