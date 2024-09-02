##  Android 통신 라이브러리

### ✔ OkHttp

- OkHttp는 Square에 의해 개발된 오픈 소스 HTTP 클라이언트 라이브러리입니다.
- REST API 및 서버와 HTTP 기반의 클라이언트의 요청과 응답에 편의성 제공을 위한 목적으로 개발되었습니다.
- 이 라이브러리는 *Connection pooling과 Redirection을 도입해 접속을 더 안정적이게 하면서도, 속도를 개선시킬 수 있는 여러가지 기술이 적용되었습니다.

#### 특징

- *Okio와 코틀린으로 구성되어있습니다.
- *Connection pooling과 Redirection을 도입해 더 안정적이고 개선된 속도로 HTTP 통신을 가능하게 합니다.
- OkHttp는 통신을 동기화로할지 비동기 처리로할지 선택할 수 있습니다, 하지만 스레드를 넘나들 수 없으므로 Handler를 사용합니다.

#### 예제

```kotlin
// Get URL
OkHttpClient client = new OkHttpClient();

String run(String url) throws IOException {
  Request request = new Request.Builder()
      .url(url)
      .build();

  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}

// Post to a Server
public static final MediaType JSON = MediaType.get("application/json; charset=utf-8");

OkHttpClient client = new OkHttpClient();

String post(String url, String json) throws IOException {
  RequestBody body = RequestBody.create(json, JSON);
  Request request = new Request.Builder()
      .url(url)
      .post(body)
      .build();
  try (Response response = client.newCall(request).execute()) {
    return response.body().string();
  }
}
```

### ✔ Retrofit

- Retrofit은 OkHttp를 개발한 Square에서 2013/05/14 에 발표한 라이브러리입니다. HttpURLConnection을 사용하기 편하도록 랩핑한게 Volley라면 **Retrofit은 OkHttp를 랩핑한 것**입니다.
- 내부적으로는 OkHttp를 사용하여 HTTP 요청을 처리합니다. RESTful API를 쉽게 호출하고 결과를 자바 객체에 매핑할 수 있습니다.

#### 특징

- 타입 세이프(Type safe): Java나 Kotlin의 인터페이스와 어노테이션을 사용해 API를 정의하므로, 컴파일 시간에 오류를 잡을 수 있습니다.
- 비동기 및 동기 요청 지원: **Call 또는 RxJava, Kotlin Coroutines**와 같은 도구를 사용하여 **비동기 처리**를 쉽게 할 수 있습니다.
- 자동 JSON 파싱: **GSON, Jackson, Moshi 등 다양한 JSON 라이브러리와 연동**이 가능합니다.
- 코드 최적화: Retrofit은 **어노테이션을 사용하여 코드를 생성**하기 때문에 API 호출 코드가 매우 간결하고 가독성이 높아, 유지보수가 쉽습니다.

#### 예제

```kotlin

interface ApiService {
    @GET("users/2")
    fun getUser(): Call<User>
}

data class User(
    val id: Int,
    val name: String,
    val email: String
)

// Activity
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val retrofit = Retrofit.Builder()
            .baseUrl("https://jsonplaceholder.typicode.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .build()

        val apiService = retrofit.create(ApiService::class.java)

        apiService.getUser().enqueue(object : Callback<User> {
            override fun onResponse(call: Call<User>, response: Response<User>) {
                if (response.isSuccessful) {
                    val user = response.body()
                    // TODO: user 객체를 이용한 작업
                }
            }

            override fun onFailure(call: Call<User>, t: Throwable) {
                // TODO: 에러 처리
            }
        })
    }
}
```

## ✅ OkHttp 와 Retrofit의 비교

### 공통적인 특징

- Square에서 만든 **HTTP 통신 라이브러리**입니다.
- OkHttp를 래핑하여 더 Type-safe하고, 더 직관적으로 사용할 수 있도록 인터페이스로 만들어진 게 Retrofit입니다. 따라서 **근본적인 HTTP 통신 구조는 동일**합니다.

### 차이점

- **추상화 레벨**:
    - OkHttp: **낮은 수준의 라이브러리**로, HTTP 요청과 응답을 생성하고 처리하는 것이 비교적 복잡할 수 있습니다.
    - Retrofit: **높은 수준의 추상화**를 제공하며, **어노테이션을 사용하여 API 엔드포인트를 인터페이스로 정의**할 수 있습니다.
- **Response Handling**:
    - OkHttp: **응답을 직접 처리**해야 하며, JSON 파싱 등의 작업도 수동으로 해야 할 수 있습니다.
        - enqueue를 사용하면 네트워크 호출이 **자동으로 백그라운드**에서 수행되지만, **Thread 변경 기능이 제공되지 않기**에 메인스레드에서 데이터를 업데이트 하기 위해 **runOnUiThread**를 사용해야합니다.
    - Retrofit:
        - **GSON, Jackson 등의 라이브러리**와 쉽게 통합할 수 있어 자동으로 JSON을 **Java/Kotlin 객체로 변환**해줍니다.
        - Retrofit의 경우 **enqueue를 사용하면 네트워크 호출이 백그라운드**에서 이루어지며, **자동으로 메인스레드에 전달**됩니다.
- **용도**:
    - OkHttp: 단순한 API 호출부터 웹 소켓, HTTP/2 등과 같은 복잡한 네트워크 작업까지 수행하는데에 사용됩니다.
    - Retrofit: 주로 REST 또는 GraphQL과 같은 API에 요청을 보내는 작업에 최적화되어 있습니다.

> 🔫 **정리하자면**  
> Retrofit이 OkHttp보다 좋은점은,
> 
> - 어노테이션(Annotation) 사용으로 코드의 가독성이 좋고, 직관적인 설계가능.
> - 통신 결과값을 JSON으로 변환해줄 필요가 없음
> - 결과값을 메인스레드에서 바로 사용할 수 있음

> 🌟 _**BUT**_ **둘을 같이 쓰는 이유**는,  
> **OkHttp의 Interceptor 기능**: OkHttp 라이브러리는 HTTP 호출을 중간에서 가로챌 수 있는 Interceptor 기능을 제공합니다. 이를 통해 API 호출 전후에 특정 로직을 실행할 수 있으며, 예를 들어 로깅, 헤더 추가, 캐싱 정책 설정 등이 가능합니다.  
> **서버 통신 시간 조절**: OkHttp를 사용하면 네트워크 호출의 타임아웃 시간을 조절할 수 있습니다. 이는 서버와의 통신을 더욱 안정적으로 만들어 줍니다.  
> **Retrofit은 이러한 OkHttp의 기능을 더욱 쉽게 사용할 수 있게 해주며**, 자동으로 JSON 또는 다른 데이터 형식을 자바 객체로 변환해주는 기능, API 호출의 간편화 등 다양한 추가 기능을 제공합니다.

> 📝 **결과적으로**, OkHttp의 세밀한 네트워크 제어 기능과 Retrofit의 사용자 친화적인 API 호출 기능을 결합하여 **최고의 성능과 편의성을 동시에 얻을 수 있습니다.** 이러한 이유로 두 라이브러리를 함께 사용하는 것이 **보편적입**니다.

## ✅ 어려운 단어 정리

- **Connection pooling**: 네트워크 연결을 효율적으로 관리하기 위한 기술입니다. HTTP 요청을 보내기 위해서는 클라이언트와 서버 간에 네트워크 연결을 맺어야 하는데, 이러한 연결을 매번 새로 생성하고 해제하는 과정은 상당히 비용이 큽니다. 연결을 맺고 끊는 데 드는 시간과 자원을 절약하기 위해, 이미 사용된 연결을 재사용할 수 있는 구조를 'Connection Pool'이라고 합니다.
    
- **Okio**: Okio는 Square사에서 개발한 자바 라이브러리로, 기본 자바 I/O 라이브러리의 한계를 극복하고 더 효율적인 I/O 작업을 위해 설계되었습니다. Okio는 특히 바이트 코드의 효율적인 읽기 및 쓰기를 위해 만들어졌으며, 그 외에도 다양한 기능을 제공합니다.





---
출처 - https://velog.io/@kej_ad/Android-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%ED%86%B5%EC%8B%A0-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%ACOkHttp-Retrofit