



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

### 고급 설정시

```xml
<filter>
  <filter-name>CorsFilter</filter-name>
  <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
  <init-param>
    <param-name>cors.allowed.origins</param-name>
    <param-value>*</param-value>
  </init-param>
  <init-param>
    <param-name>cors.allowed.methods</param-name>
    <param-value>GET,POST,HEAD,OPTIONS,PUT,DELETE</param-value>
  </init-param>
  <init-param>
    <param-name>cors.allowed.headers</param-name>
    <param-value>Content-Type,X-Requested-With,accept,Origin,Access-Control-Request-Method,Access-Control-Request-Headers</param-value>
  </init-param>
  <init-param>
    <param-name>cors.exposed.headers</param-name>
    <param-value>Access-Control-Allow-Origin,Access-Control-Allow-Credentials</param-value>
  </init-param>
  <init-param>
    <param-name>cors.support.credentials</param-name>
    <param-value>true</param-value>
  </init-param>
  <init-param>
    <param-name>cors.preflight.maxage</param-name>
    <param-value>10</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CorsFilter</filter-name>
  <url-pattern>/*</url-pattern>
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


# Proxy 적용


## Proxy

CORS 처리를 백엔드 개발자에게 요청할 필요없이 React 라이브러리, Webpack Dev Server에서 제공하는 proxy기능을 사용하면 CORS 정책을 우회할 수 있다. 이는 별도의 응답 헤더를 받을 필요 없이 브라우저는 React 앱으로 데이터를 요청하고, 해당 요청을 백엔드로 전달하게 됩니다. 여기서 React 앱이 서버로부터 받은 응답 데이터를 다시 브라우저로 전달하는 방법을 쓰기 때문에 브라우저는 CORS 정책을 위반했는지 모르게 됩니다. 브라우저를 proxy 기능을 통해 속이는 것입니다.

![[img1.daumcdn.webp]]



위 그림에서 Direct data exchange는 proxy를 사용하기 전의 흐름이다.

프론트엔드, 즉 여러분이 개발한 React 앱에서 브라우저 쪽으로 요청을 보냅니다. 그러면 브라우저는 백엔드, 즉 서버 쪽으로 리소스를 요청하게 됩니다. 이때 접근 권한이 있는지, 즉 출처가 같은지 확인하는데 이때 백엔드 서버는 정상적으로 200 OK 응답을 브라우저에게 보냅니다. 마지막으로 브라우저는 받은 리소스 및 응답과 함께 출처가 같은지 아닌지 확인하게 되는데, 이때 출처가 다르다면 응답을 파기(CORS Error) 하고, 출처가 같다면 응답을 파기하지 않고 다시 프론트엔드 쪽으로 응답을 보내주는 것입니다.

하지만 아래부분인 data exchange through a proxy server는 proxy를 적용해 브라우저를 속인 후 흐름입니다. React 앱에서 브라우저를 통해 API를 요청할 때, proxy를 통해 백엔드 서버로 요청을 우회하여 보내게 됩니다. 그러면 백엔드 서버는 응답을 React 앱으로 보내고, React 앱은 받은 응답을 백엔드 서버 대신 브라우저에게 전달합니다. 이렇게 되면 출처가 같아지기 때문에 브라우저는 이 사실을 눈치채지 못하고 허용하게 됩니다.

## [Proxy 사용방법](https://jhbljs92.tistory.com/entry/Proxy-CORS-%EC%A0%95%EC%B1%85-%EC%9A%B0%ED%9A%8C#Proxy%20%EC%82%AC%EC%9A%A9%EB%B0%A9%EB%B2%95-1)

### [1. webpack dev server의 proxy 기능 사용](https://jhbljs92.tistory.com/entry/Proxy-CORS-%EC%A0%95%EC%B1%85-%EC%9A%B0%ED%9A%8C#1.%20webpack%20dev%20server%EC%9D%98%20proxy%20%EA%B8%B0%EB%8A%A5%20%EC%82%AC%EC%9A%A9-1)

먼저 webpack dev server에서 제공하는 proxy 기능을 사용하는 방법이 있습니다. webpack dev server의 proxy를 사용하게 되면, 브라우저 API를 요청할 때 백엔드 서버에 직접적으로 요청을 하지 않고, 현재 개발서버의 주소로 우회 요청을 하게 됩니다. 그러면 웹팩 개발 서버에서 해당 요청을 받아 그대로 백엔드 서버로 전달하고, 백엔드 서버에서 응답한 내용을 다시 브라우저 쪽으로 반환합니다.

1-1. 클라이언트 폴더 **package.json**의 맨 밑에 프록시 설정을 추가.

데이터를 받아올 서버의 주소가 localhost:3080이므로 아래와 같이 서버의 주소를 넣어준다.




![[Pasted image 20241022150508.png]]



1-2. 그다음 fetch함수를 도메인 주소를 지우고 파라미터만 남겨둔다.



![[Pasted image 20241022150516.png]]



위처럼 요청 url에 /api/books, /api/book 처럼 도메인주소를 지우고 파라미터만 남겨주었다.

이렇게 하면 요청을 보낼 때 현재페이지의 도메인으로 인식하게 되어 http://localhost:3080/api/books, http://localhost:3080/api/book 로 요청이 보내진다.

**이렇게 수정을 했다면 반드시 서버와 앱을 껐다 키자!!**

위와 같은 방법으로 설정하는 방법은 CRA로 만든 프로젝트에서는 유효하지 않기 때문에

리액트 라이브러리 http-proxy-middleware를 불러와 처리해야한다.

### [2. http-proxy-middleware 라이브러리](https://jhbljs92.tistory.com/entry/Proxy-CORS-%EC%A0%95%EC%B1%85-%EC%9A%B0%ED%9A%8C#2.%20http-proxy-middleware%20%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-1)

webpack dev server에서 제공하는 proxy는 전역적인 설정이기 때문에, 종종 해당 방법이 충분히 적용되지 않는 경우가 생기기도 합니다. 그래서 수동으로 proxy를 적용해줘야 하는 경우가 있는데, 이때는 http-proxy-middleware 라이브러리를 사용해야 합니다.

2-1. 클라이언트 폴더에서 아래의 명령어로 라이브러리 설치

```bash
npm install http-proxy-middleware --save
```

2-2. 그리고 React App의 src 파일 안에서 **setupProxy.js** 파일을 생성하고, 안에서 설치한 라이브러리 파일을 불러온 다음, 아래와 같이 작성을 합니다.



![[Pasted image 20241022150523.png]]


파일명은 무조건 **setupProxy.js**로 해야한다.

하지만 받아올 서버가 여러개라면 두가지 방법으로 작성해 줄 수 있다.

**첫번째 방법: 여러개의 app.use()로 각각 처리**


![[Pasted image 20241022150529.png]]


**두번째 방법: 라우터 이용하기**


![[Pasted image 20241022150534.png]]



---




### `@CrossOrigin`을 사용하는 경우

- **목적**: 클라이언트(프론트엔드)에서 다른 출처의 API에 요청할 수 있도록 허용하는 것입니다.
- **작동 방식**: 이 어노테이션을 통해 서버는 특정 출처의 요청을 수락하게 됩니다. 즉, 브라우저가 다른 출처의 API를 호출할 때 CORS 정책에 의해 차단되지 않도록 합니다.
- **예시**: `@CrossOrigin(originPatterns = "http://localhost:8080")`이 있으면, 해당 URL에서의 요청이 서버에 전달됩니다. 하지만 이 방식은 프록시 역할을 하지 않고, 단순히 허용하는 것입니다.

### 2. 프록시 컨트롤러를 만드는 경우

- **목적**: 외부 API에 대한 요청을 중계하는 역할을 합니다. 클라이언트가 직접 외부 API에 요청하지 않고, 서버(백엔드)가 대신 요청하여 그 결과를 클라이언트에 전달합니다.
- **작동 방식**: 클라이언트는 서버의 프록시 엔드포인트로 요청을 보내고, 서버는 외부 API에 요청한 후 그 응답을 클라이언트에 반환합니다. 이 과정에서 서버가 요청을 변환하거나 추가적인 처리를 할 수 있습니다.
- **예시**: `ProxyController`에서 `/proxy/crawl`로 요청을 받아 외부 URL로 요청을 전송하고, 받은 데이터를 가공해서 클라이언트에게 전달합니다.

### 차이점 요약

|요소|`@CrossOrigin` 사용|프록시 컨트롤러 생성|
|---|---|---|
|**목적**|CORS 정책 설정|외부 API 요청 중계|
|**작동 방식**|클라이언트가 직접 외부 API에 접근 가능|서버가 외부 API에 요청을 대신 수행|
|**장점**|설정이 간단하고 빠르게 적용 가능|요청을 변환하거나 추가 로직 적용 가능|
|**단점**|CORS 오류 발생 가능|서버 부하 증가, 응답 시간 증가 가능|

```java
@Controller
public class ProxyController {
    @GetMapping("/proxy/crawl")
    public ResponseEntity<String> proxyCrawlRequest(@RequestParam String url) {
        RestTemplate restTemplate = new RestTemplate();
        try {
            ResponseEntity<byte[]> response = restTemplate.getForEntity(url, byte[].class);
            String body = new String(response.getBody(), StandardCharsets.UTF_8);
            HttpHeaders headers = new HttpHeaders();
            headers.setContentType(new MediaType("application", "rss+xml", StandardCharsets.UTF_8));
            return new ResponseEntity<>(body, headers, HttpStatus.OK);
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.BAD_GATEWAY).body("데이터를 가져오지 못했습니다: " + e.getMessage());
        }
    }
}

```




---
출처 - https://velog.io/@soyeon207/%EC%8B%A4%EC%8A%B5%ED%8E%B8-CORS-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0

https://javaoop.tistory.com/87