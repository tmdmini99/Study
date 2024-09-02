**OkHttp**는 REST API, HTTP 통신을 간편하게 사용할 수 있도록 만들어진 자바 라이브러리다. **"Square"**라는 회사가 만든 OkHttp 라이브러리는 어쩌면 더 잘 알려져있는 **Retrofit**이라는 라이브러리의 기본이 된다. OkHttp 라이브러리를 이용하면 간편하게 몇 줄의 코드로 REST API, HTTP 기반의 요청, 응답을 처리할 수 있다.


![[Pasted image 20240902160517.png]]


OkHttp 라이브러리는 오픈소스로 공개된 소프트웨어다. (링크 : [OkHttp github](https://github.com/square/okhttp)) 문제가 생기거나 내부 동작이 궁금하면 코드를 열어볼 수 있다. 

## OkHttp 메이븐(Maven) 의존성 설정

아마도 메이븐 프로젝트를 많이 사용할 텐데, 메이븐 프로젝트에서 OkHttp를 사용하기 위해서는 pom.xml 파일에 다음 의존성을 추가하면 된다. 버전은 적당히 최신 버전이나 써야하는 특정 버전을 입력하면 된다.

```xml
<dependency>
  <groupId>com.squareup.okhttp3</groupId>
  <artifactId>okhttp</artifactId>
  <version>3.10.0</version>
</dependency>
```

메이븐 말고 그래들(Gradle) 같은 빌드 도구를 이용하고 싶으면, 

```xml
implementation group: 'com.squareup.okhttp', name: 'okhttp', version: '2.7.5'
```

이런 식으로 입력하면 된다.


![[Pasted image 20240902160524.png]]

## REST API 예제

REST API는 일반적으로 다음과 같은 형태로 제공된다.

```
http://127.0.0.1:8080/v1/test/function
```

**\http://127.0.0.1:8080/v1/test/function**이라는 URL로 REST API가 제공되면, 이 곳에 GET, HEAD, DELETE, POST, PUT 요청을 보내서 적당한 서비스를 받을 수 있다.

이제 이 REST API를 사용하는 Java 코드를 작성해 보자. 

## GET, HEAD, DELETE 예제

GET, HEAD, DELETE는 동일한 형태로 사용할 수 있다. 우선 정보를 가져오는 GET을 기준으로 살펴보겠다.  **\http://127.0.0.1:8080/v1/test/function** URL에 GET 요청을 날려서 정보를 얻어오는 예제다. 우리가 잘 알고 있는 curl 명령을 터미널에서 다음과 같이 실행할 수 있다.

```jsp
$ curl -v -H "password: BlahBlah" -X GET http://127.0.0.1:8080/v1/test/function?key=123
```

REST API URL 끝에 **key=123**이라는 파라미터를 같이 넘겨준 것을 볼 수 있다. 이 키를 이용해서 DB를 조회한 다음 정보를 정제해서 넘겨주는게 일반적인 패턴이다.

-H 옵션을 이용해서 **"passworkd: BlahBlah"**라는 값을 헤더로 넘겼다. 커버러스 인증을 하지 않으면 비밀번호를 입력해서 REST API 사용을 제한하는 경우가 많은데, curl 명령 사용시 -H 옵션을 이용해서 HTTP 헤더에 비밀번호 값을 달아서 보내준다.

이런 동작을 OkHttp를 이용한 자바 프로그램으로 구현해보자.

```java
public boolean getUserInfo(String key) {
 
    try {
        String url = "http://127.0.0.1:8080/v1/test/function?key=" + key;
 
        // OkHttp 클라이언트 객체 생성
        OkHttpClient client = new OkHttpClient();
 
        // GET 요청 객체 생성
        Request.Builder builder = new Request.Builder().url(url).get();
        builder.addHeader("password", "BlahBlah");
        Request request = builder.build();
 
        // OkHttp 클라이언트로 GET 요청 객체 전송
        Response response = client.newCall(request).execute();
        if (response.isSuccessful()) {
            // 응답 받아서 처리
            ResponseBody body = response.body();
            if (body != null) {
                System.out.println("Response:" + body.string());
            }
        }
        else
            System.err.println("Error Occurred");
 
        return true;
    } catch(Exception e) {
        e.printStackTrace();
    }
    
    return false;
}
```

전체 소스코드는 위와 같다. 코드를 하나씩 살펴보겠다.

```java
OkHttpClient client = new OkHttpClient();
```

우선 OkHttpClient 객체를 생성한다.

```java
Request.Builder builder = new Request.Builder().url(url).get();
builder.addHeader("password", "BlahBlah");
Request request = builder.build();
```

REST API 서버로 전송할 GET 요청 객체를 생성한다. Request.Builder의 **get() 메소드**를 이용해서 GET 요청을 위한 빌드 작업임을 명시한다. **addHeader() 메소드**는 curl 명령의 -H 옵션에 해당한다. 비밀번호를 여기에 입력하면된다.

마지막으로 build() 메소드를 이용해서 Request 객체를 생성한다.

```java
Response response = client.newCall(request).execute()
```

만들어진 객체를 newCall() 메소드를 이용해서 REST API 서버로 전송한다. 이 때, **execute() 메소드를 꼭 호출**해줘야한다. 이 메소드를 호출하지 않으면 서버로 객체가 전송되지 않는다. (이 메소드 빼먹고 한참 소스코드 까보고 있었다;;;) execute() 메소드는 서버에서 요청에 대한 처리가 완료되고 응답이 올 때까지 기다렸다가 리턴된다.

execute() 메소드는 Response 객체를 반환한다. Response 객체에는 사용자가 던진 GET 요청의 처리 결과에 대한 정보들이 담겨있다. 어떤 정보를 요청했으면, 그 정보가 들어있다. 만약 에러가 발생했다면 에러에 대한 정보들도 담겨있다.

```java
if (response.isSuccessful()) {
    ResponseBody body = response.body();
    if (body != null) {
        System.out.println("Response:" + body.string());
    }
}
```

Response 객체에는 요청이 성공적으로 처리되었는지 확인할 수 있는 **isSuccessful()** 메소드가 있다. 성공했다면 body() 메소드를 이용해서 GET 요청에 대한 정보들을 확인할 수 있다.

```java
Request.Builder headBuilder = new Request.Builder().url(url).head();
Request.Builder deleteBuilder = new Request.Builder().url(url).delete();
```

HEAD 요청과 DELETE 요청은 기본적으로 GET 요청과 동일하게 처리하면 된다. 다만 Request.Builder 에서 get() 메소드 대신 head(), delete() 메소드를 호출하면 된다.

## PUT, POST 예제

POST 요청과 PUT 요청은 리소스를 생성하거나 수정하는 역할을 한다. GET 요청과 다르게 데이터 값을 함께 포함시켜서 요청을 보내야 하는 경우가 많다. 마찬가지로 POST 요청을 기준으로 살펴보겠다.

**\http://127.0.0.1:8080/v1/test/function** URL에 POST 요청을 날려서 정보를 생성하는 동작을 살펴보자. 터미널에서 curl 명령을 이용하면 다음처럼 쓰게 된다. 

```
$ curl -v -H "password: BlahBlah" -X POST http://127.0.0.1:8080/v1/test/function -d "{\"key\": 123, \"value\":55323}"
```

GET 요청때와 마찬가지로 -H 옵션으로 비밀번호를 입력한다. 다른 점은 -d 옵션을 이용해서 POST 요청의 데이터를 함께 전송했다는 점이다. 

이 동작을 자바 프로그램으로 작성해보면,

```java
public boolean postUserInfo() {
 
    try {
        String url = "http://127.0.0.1:8080/v1/test/function";
        String postBody = "" + "{" + "\"key\":123," + "\"value\":55323" + "}";
 
        // OkHttp 객체 생성
        OkHttpClient client = new OkHttpClient();
        
        // RequestBody 생성
        RequestBody requestBody = RequestBody.create(
                 MediaType.parse("application/json; charset=utf-8"), postBody);
 
        // Post 객체 생성
        Request.Builder builder = new Request.Builder().url(url)
            .addHeader("Password", "BlahBlah")
            .post(requestBody);
        Request request = builder.build();
 
        // 요청 전송
        Response response = client.newCall(request).execute();
        if (response.isSuccessful()) {
            // 응답 Body
            ResponseBody body = response.body();
            if (body != null) {
                System.out.println("Response:" + body.string());
            }
        } else
            System.err.println("Error Occurred");
 
        return true;
    } catch (Exception e) {
        e.printStackTrace();
    }
 
    return false;
}
```

전체적인 코드 사용은 GET 요청과 비슷하다. 하지만 이번에는 POST 요청을 위한 RequestBody 객체라 추가되었다.

```java
RequestBody requestBody = RequestBody.create(
                 MediaType.parse("application/json; charset=utf-8"), postBody);
```

POST 요청을 보내서 생성할 데이터를 JSON 포맷으로 표현했다. 이 MIME 타입과 데이터를 create() 메소드에 넣어주면 RequestBody 객체가 생성된다.

```java
Request.Builder builder = new Request.Builder().url(url)
            .addHeader("Password", "BlahBlah")
            .post(requestBody);
Request request = builder.build();
```

이 RequestBody 객체를 post() 메소드의 인자로 넣어서 POST 요청 객체를 만든다. 

```java
RequestBody body = RequestBody.create(null, new byte[0]);
```

만약 전송할 RequestBody가 없거나 사용되지 않는다면, 위 코드를 이용해서 비어있는 RequestBody 객체를 생성해도 된다.

이후 REST API 서버로 전송하고 응답을 받아서 처리하는 과정은 GET 요청과 동일하다. 마찬가지로 PUT 요청 역시 POST 요청처럼 처리하면된다.

## 비동기 요청

지금까지의 예제에서 execute() 메소드로 요청을 REST API 서버로 전송하면 블러킹되었다. 요청에 대한 응답이 올 때까지 코드의 진행이 멈추고 기다렸다. 이런 동기적인 구성은 매우 간단하지만 동시에 비효율적이기도 하다. 요청을 던져놓고 응답이 올때까지 다른 일을 하면 더 효율적이다.

**REST API를 비동기적(Asynchronous)으로 사용하려면 execute() 메소드 대신 enqueue() 메소드를 사용**하면된다. 이 때, 요청이 완료되면 실행될 콜백(Callback)을 함께 넘겨주면, 응답이 왔을 때 콜백이 실행된다.

Callback은 인터페이스로 다음 메소드들을 구현해야한다.

```java
public interface Callback {
 
    void onFailure(Call call, IOException e);
 
    void onResponse(Call call, Response response) throws IOException;
}
```

각각 실패했을 때와 성공했을 때의 처리를 담당하는 메소드임을 알 수 있다.

위에서 봤던 POST 요청 전송 예제를 비동기적으로 구현하면 다음과 같다.

```java
public void postAsyncUserInfo() {
 
    try {
        String url = "http://127.0.0.1:8080/v1/test/function";
        String postBody = "" + "{" + "\"key\":123," + "\"value\":55323" + "}";
 
        OkHttpClient client = new OkHttpClient();
        RequestBody requestBody = RequestBody.create(
                        MediaType.parse("application/json; charset=utf-8"), postBody);
 
        Request.Builder builder = new Request.Builder().url(url)
                                                .addHeader("Password", "BlahBlah")
                                                .post(requestBody);
        Request request = builder.build();
 
        client.newCall(request).enqueue(new Callback() {
 
            @Override
            public void onFailure(Call call, IOException e) {
                System.err.println("Error Occurred");
            }
 
            @Override
            public void onResponse(Call call, Response response) throws IOException {
                ResponseBody body = response.body();
                if (body != null) {
                    System.out.println("Response:" + body.string());
                }
            }
        });
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

Callback 인터페이스를 익명 클래스로 구현해서 사용했다.

OkHttp 라이브러리를 이용하면 몇 줄의 코드로 쉽게 REST API를 사용 할 수 있다.





---
출처 - https://hbase.tistory.com/90