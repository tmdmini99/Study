



**/WEB-INF/web.xml 파일에 코드추가**

```xml
<filter>

    <filter-name>CorsFilter</Filter-name>

    <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>

    <init-param>

        <param-name>cors.allowed.origins</param-name>

        <param-value>*</param-value>

    </init-param>

</filter>

<filter-mapping>

    <filter-name>CorsFilter</filter-name>

    <url-pattern>*</url-pattern>

</filter-mapping>
```



## CORS 적용

### ① @CrossOrigin 추가



- spring 4.2 버전부터 지원
- Controller 혹은 method 단위로 적용할 수 있다

```java
@CrossOrigin(origins = "*", allowedHeaders = "*")
```

|설정|설명|
|---|---|
|origins|허용할 도메인, 여러개인 경우 origins = {"[http://localhost:8000"](http://localhost:8000%22/), "[http://localhost:8001"}](http://localhost:8001%22%7D/) 이렇게 표현|
|allowedHeaders|허용할 헤더|
|maxAge|preflight 결과를 캐시에 저장할 시간 , default 값은 1800초 (30분)|

**예제 적용**

```java
@CrossOrigin(origins = "http://localhost:8001") // 🌟 추가
@RestController
@RequestMapping("/test")
public class DemoController {

    @GetMapping
    public String test() {
        return "테스트입니다.";
    }

}
```

Controller 에 @CrossOrigin 을 추가했을 때 정상적으로 실행되는 걸 확인 할 수 있습니다.


![[Pasted image 20241022093844.png]]



### . WebConfig 에 CORS 설정

WebMvcConfigurer 를 상속받은 WebConfig 파일을 만들어주고 @Configuration 를 통해 환경 설정 파일로 설정

|설정|설명|
|---|---|
|addMapping|프로그램에서 제공하는 CORS 를 허용해줄 도메인, 해당 코드에서는 모든 URL 허용|
|allowedOrigins|요청을 허용할 도메인, 해당 코드에서는 프론트 도메인 URL|
|allowedHeaders|요청을 허용할 헤더|
|allowedMethods|허용할 HTTP 메서드|
|allowCredentials|쿠키 요청을 허용|
|maxAge|preflight 요청에 대한 응답을 브라우저에서 캐싱하는 시간|

**예제 적용**  
WebMvcConfigurer 를 상속받은 WebConfig Configuration 파일을 만들고, 서버의 모든 API (`.addMapping("/**")`) 를 localhost:8001 에서 접속할 수 있도록 설정 (`.allowedOrigins("http://localhost:8001")`)

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:8001");
    }
}
```

# 프론트에서 CORS 적용하기

### Proxy 만들기

1. 프론트(localhost:8001) 에서 요청을 보냄
2. 서버로 바로 가기 전 프록시 서버를 통함  
    프록시 서버에서 `Access-Control-Allow-Origin : *` 를 세팅해서 응답해줌
3. 서버(localhost:8000) 에서 응답을 보내고 응답 헤더가 `Access-Control-Allow-Origin : *` 이기 때문에 CORS 정책을 위반하지 않음

**① react 의 경우**  
Webpack 에서 Proxy 기능 지원하기 때문에

package.json 에서

```null
{
  "proxy": "http://xxx"
}
```

만 추가하면 간단하게 해결 할 수 있습니다.

**② 실행중인 프록시 서버 사용**  
cors-anywhere 프록시 서버를 사용하는 방법도 있습니다.

```null
https://cors-anywhere.herokuapp.com/https://soyeon207.com
```

로 cors-anywhere URL 뒤에 요청하는 URL 을 붙여서 요청하면 되는데 헤로쿠의 프록시 서버가 중간에 응답 헤더를 설정해주는 방식으로 파워풀 하긴 하지만 속도가 느리고 현재는 정상적으로 작동하지 않음





---
출처 - https://velog.io/@soyeon207/%EC%8B%A4%EC%8A%B5%ED%8E%B8-CORS-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0

https://javaoop.tistory.com/87