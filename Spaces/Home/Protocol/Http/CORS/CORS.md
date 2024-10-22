# CORS의 등장 배경

## SOP와 CORS

브라우저는 기본적으로 SOP 정책을 따르고 있다.  
SOP는 2011년 RFC 6454에서 등장한 보안 정책으로 **"같은 출처에서만 리소스를 공유할 수 있다"**라는 규칙을 가진 정책이다.

그러나 보안도 중요하지만 개발을 하다 보면 기능상 어쩔 수 없이 다른 출처 간의 상호작용을 해야 하는 케이스가 존재한다.  
'다른 출처'의 API 서버를 두거나, '다른 출처'의 외부 리소스를 가져다 쓰는 경우가 있기 때문이다.

이런 경우를 대비 하여 몇 가지 예외 조항을 두고, 이 조항에 해당하는 리소스 요청은 출처가 다르더라도 허용하기로 했다.  
그 중 하나가 **'CORS 정책을 지킨 리소스 요청'**이다.  
즉 CORS라는 정책에 위반되지 않으면 정상적으로 리소스를 요청할 수 있게 해준다는 뜻이다.

## 왜 이런 보안 정책이 등장한 것일까?

> 출처가 다른 두 개의 어플리케이션이 마음대로 소통할 수 있는 환경은 꽤 위험한 환경이기 때문이다.  
>   
> **애초에 클라이언트 어플리케이션, 특히나 웹에서 돌아가는 클라이언트 어플리케이션은 사용자의 공격에 너무나도 취약한 친구라는 사실을 잊지말자**. 당장 브라우저의 개발자 도구만 열어도 DOM이 어떻게 작성되어있는지, 어떤 서버와 통신하는지, 리소스의 출처는 어디인지와 같은 각종 정보들을 아무런 제재없이 열람할 수 있지 않은가?  
>   
> 최근에는 자바스크립트 소스 코드를 난독화해서 읽기 어렵다고 하지만, 난독화는 어디까지나 난독화일 뿐이지 암호화가 아니다. 그리고 아무리 난독화되어있다고 해도 사람이 바로 이해할 수 없는 정도도 아닌데다가, 소스 코드를 직접 볼 수 있다는 것 자체가 보안적으로 상당히 취약한 부분이다.  
>   
> 이런 상황 속에서 다른 출처의 어플리케이션이 서로 통신하는 것에 대해 아무런 제약도 존재하지 않는다면, 악의를 가진 사용자가 소스 코드를 쓱 구경한 후 **[CSRF(Cross-Site Request Forgery)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)나 [XSS(Cross-Site Scripting)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)**와 같은 방법을 사용하여 여러분의 어플리케이션에서 코드가 실행된 것처럼 꾸며서 **사용자의 정보(토큰이나 쿠키 등)를 탈취하기가 너무나도 쉬워진다**  
>   


  
  

그래서 개발을 할 때 발생하는 SOP의 불편함을 해소하면서, 보안을 지키기 위해 등장한 것이 CORS(Cross-Origin Resource Sharing)이다.

**_"CORS를 지키면, '다른 출처'에서도 데이터를 불러올 수 있게 해줄게!"_  
**

  
  
  

# CORS란?

## CORS(Cross-Origin Resource Sharing)

한국어로 직역하면 '교차 출처 리소스 공유'라고 해석할 수 있다. 여기서 '교차 출처(Cross-Origin)'이란 '다른 출처'를 의미한다.  
해석을 해보면 Cross-Origin의 Resrouce를 공유하는 정책이라고 볼 수 있다.

즉, **CORS란 도메인이 다른 서버끼리 리소스를 주고 받을 때 보안을 위해 설정된 정책**이라고 생각하면 된다.

예를 들어, 웹 사이트 A가 API 서버 B에서 데이터를 가져오려 할 때,  
API 서버 B에서 CORS 허용 설정이 되어 있지 않으면 웹 브라우저에서 API 접근이 거부될 수 있다.

또한 프론트엔드와 백엔드가 협업하면서 각자 따로 서버를 띄우게 되었을 경우도 마찬가지다. 서로 다른 React 서버(3000 포트)와 Springboot(8080 포트) 서버가 리소스를 주고 받으려 한다면 포트가 달라 서로 다른 출처로 판단되어 CORS 위반 에러가 발생한다.

[CORS이란? / 설정 확인 / 예제 정리](https://sh-safer.tistory.com/326)

  
  

### Origin

그렇다면 같은 출처와 다른 출처의 구분은 어떻게 해줄까?  
그 전에 Origin(출처)에 대해 짚고 넘어가야 한다.



![[Pasted image 20241022090658.png]]



> 서버의 위치를 의미하는 **`https://google.com`**과 같은 URL들은 마치 하나의 문자열 같아 보여도, 사실은 여러 개의 구성 요소로 이루어져있다.  
>   
> 이때 출처는 **`Protocol`**과 **`Host`**, 그리고 위 그림에는 나와있지 않지만 **`:80`**, **`:443`**과 같은 포트 번호까지 모두 합친 것을 의미한다. 즉, 서버의 위치를 찾아가기 위해 필요한 가장 기본적인 것들을 합쳐놓은 것이다.  
>   
> **또한 출처 내의 포트 번호는 생략이 가능한데, 이는 각 웹에서 사용하는 `HTTP`, `HTTPS` 프로토콜의 기본 포트 번호가 정해져있기 때문이다. (port 80)**  
>   
> 그러나 만약 **`https://google.com:443`**과 같이 출처에 포트 번호가 명시적으로 포함되어 있다면 **이 포트 번호까지 모두 일치해야 같은 출처라고 인정된다.**  
> 하지만 이 케이스에 대한 명확한 정의가 표준으로 정해진 것은 아니기 때문에, 더 정확히 이야기하자면 **어떤 경우에는 같은 출처, 또 어떤 경우에는 다른 출처로 판단될 수도 있다.** (진리의 케바케)  
>   


  
  

### Cross-Origin (다른 출처) 판단 기준

이제 같은 출처와 다른 출처의 구분을 판단하는 법에 대해 알아보자.  
두 개의 출처가 서로 같다고 판단하는 로직은, 두 URL의 구성 요소 중 **`Scheme(프로토콜)`**, **`Host(도메인)`**, **`Port`**, 이 3가지만 동일하면 된다.


![[Pasted image 20241022090710.png]]


`https://evan-moon.github.io:80` 라는 출처를 예로 들면  
https:// 이라는 스킴에 evan-moon.github.io 호스트를 가지고  
:80번 포트를 사용하고 있다는 것만 같다면 나머지는 전부 다르더라도 같은 출처로 인정이 된다는 것이다.



따라서 일반적으로는 **same-origin**이란 scheme(프로토콜), host(도메인), 포트가 같다는 말이며, 이 3가지 중 하나라도 다르면 **cross-origin**이다.

  
  

그러나 각 브라우저들의 독자적인 출처 비교 로직을 따라가는 경우도 있다.  
**예) `https://evan-moon.github.io` 와 `https://evan-moon.github.io:8000`  
-> 브라우저의 구현에 따라 다름  
**

> 만약 출처에 [https://evan-moon.github.io:80처럼](https://evan-moon.github.io:80%EC%B2%98%EB%9F%BC/) 포트 번호가 명시되어 있다면 명백하게 다른 출처로 인정되는 부분이지만, 예시로 든 출처의 경우 포트 번호가 포함되지 않았기 때문에 판단하기가 애매하다. RFC 6454의 Comparing Origins 섹션에는 “만약 출처가 스킴/호스트/포트의 삼중 체계라면…” 이라는 전제가 붙어있기 때문에 어떻게 해석하냐에 따라 구현이 달라질 수 있기 때문이다.  
> 그래서 이런 경우에는 각 브라우저들의 독자적인 출처 비교 로직을 따라가게 된다.

  
  
  

### CORS 정책은 언제 검사할까?



![[Pasted image 20241022090808.png]]




> 여기서 중요한 사실 한 가지는 이렇게 **출처를 비교하는 로직이 서버에 구현된 스펙이 아니라 브라우저에 구현되어 있는 스펙**이라는 것이다.
> 
> 만약 우리가 CORS 정책을 위반하는 리소스 요청을 하더라도 해당 서버가 같은 출처에서 보낸 요청만 받겠다는 로직을 가지고 있는 경우가 아니라면 **서버는 정상적으로 응답을 하고, 이후 브라우저가 이 응답을 분석해서 CORS 정책 위반이라고 판단되면 그 응답을 사용하지 않고 그냥 버리는 순서인 것이다.**
> 
> **서버는 CORS를 위반하더라도 정상적으로 응답을 해주고, 응답의 파기 여부는 브라우저가 결정한다**  
> 즉, **CORS는 브라우저의 구현 스펙에 포함되는 정책**이기 때문에, 브라우저를 통하지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다.  
> 또한 **CORS 정책을 위반하는 리소스 요청 때문에 에러가 발생했다고 해도 서버 쪽 로그에는 정상적으로 응답을 했다는 로그만 남기 때문에,** CORS가 돌아가는 방식을 정확히 모르면 에러 트레이싱에 난항을 겪을 수도 있다.  
>   
>   


  
  
  

## CORS의 동작 시나리오 3가지

> 기본적으로 웹 클라이언트 어플리케이션이 다른 출처의 리소스를 요청할 때는 HTTP 프로토콜을 사용하여 요청을 보내게 되는데, 이때 브라우저는 요청 헤더에 Origin이라는 필드에 요청을 보내는 출처를 함께 담아보낸다.  
> `Origin: https://evan-moon.github.io`  
>   
> 이후 서버가 이 요청에 대한 응답을 할 때 응답 헤더의 **`Access-Control-Allow-Origin`**이라는 값에 “이 리소스를 접근하는 것이 허용된 출처”를 내려주고, 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 **`Origin`**과 서버가 보내준 응답의 **`Access-Control-Allow-Origin`**을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다.  
>   
> 기본적인 흐름은 이렇게 간단하지만, **사실 CORS가 동작하는 방식은 한 가지가 아니라 세 가지의 시나리오에 따라 변경되기 때문에** 여러분의 요청이 어떤 시나리오에 해당되는지 잘 파악한다면 CORS 정책 위반으로 인한 에러를 고치는 것이 한결 쉬울 것이다.

  
  

### 1. Simple Request

서로 다른 도메인의 서버가 리소스를 요청하는 CORS 상황이 발생했을 때, 브라우저는 다음과 같은 절차를 사용한다.


![[Pasted image 20241022090823.png]]



먼저 **일반적인 요청**에 대해서는 CORS 정책 검사를 하지 않는데, 이러한 일반적인 요청은 다음 사항에 부합되는 요청을 의미한다.

> 1. 요청의 메소드는 **`GET`**, **`HEAD`**, **`POST`** 중 하나여야 한다.
> 2. Request Header에는 다음 속성만 허용  
>     **`Accept`**, **`Accept-Language`**, **`Content-Language`**, **`Content-Type`**, **`DPR`**, **`Downlink`**, **`Save-Data`**, **`Viewport-Width`**, **`Width`**
> 3. 만약 **`Content-Type`**를 사용하는 경우에는 **`application/x-www-form-urlencoded`**, **`multipart/form-data`**, **`text/plain`**만 허용된다.
> 4. 요청에 사용된 XMLHttpRequestUpload 객체에는 이벤트 리스너가 등록되어 있지 않다. 이들은 [XMLHttpRequest.upload](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/upload) 프로퍼티를 사용하여 접근한다.
> 5. 요청에 [ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream) 객체가 사용되지 않는다.

하지만 이들은 일반적인 방법으로 웹 어플리케이션 아키텍처를 설계하게 되면 거의 충족시키기 어려운 조건들이고,  
대부분의 HTTP API는 **`text/xml`**이나 **`application/json`** 컨텐츠 타입을 가지도록 설계되기 때문에 사실상 이 조건들을 모두 만족시키는 상황을 만나기 쉽지 않아 Simple Request 경우는 흔히 보기 어렵다.  
**(= application/json은 Simple Request에 해당되지 않음 !)**

또한 애초에 저 조건에 명시된 헤더들은 진짜 기본적인 헤더들이기 때문에, 복잡한 상용 웹 어플리케이션에서 이 헤더들 외에 추가적인 헤더를 사용하지 않는 경우는 드물며 당장 사용자 인증에 사용되는 Authorization 헤더조차 위의 조건에는 포함되지 않는다.  
**(= Authorization 헤더를 사용하면 Simple Request에 해당되지 않음!)  
(= 이외에 다른 커스텀 헤더, 권한과 관련된 헤더가 있으면 Simeple Request에 해당되지 않음! )**

추가적으로 HTTP 메소드들 중 GET 이 외의 메소드나, POST 메소드에서 특정 MIME Type은 서버 데이터에 사이드 이펙트를 발생시킬 수 있기 때문에 기본적으로 브라우저는 SimpleRequest가 아닌  **Preflight Request 방식으로 요청**할 수 있도록 규제한다.

  
  

### 2. Preflight Request

**`프리플라이트(Preflight)`** 방식은 일반적으로 우리가 웹 어플리케이션을 개발할 때 가장 마주치는 시나리오이다. 이 시나리오에 해당하는 상황일 때 브라우저는 요청을 한번에 보내지 않고 **예비 요청과 본 요청으로 나누어서 서버로 전송한다.**

다시 말해 preflight Request와 simple Request는 전반적인 로직 자체는 같고, 예비 요청의 존재 유무만 다르다고 보면 된다.

따라서 앞의 Simple Request와 같은 요청이 아닌 경우 브라우저는 접근할 리소스를 가지고 있는 서버에 **preflight Request (예비 요청)을 보낸다.**

이 예비 요청에는 HTTP 메소드 중 **`OPTIONS`** 메소드가 사용된다. 예비 요청의 역할은 본 요청을 보내기 전에 **브라우저 스스로 이 요청을 보내는 것이 안전한지 확인하는 것**이다.

이후 OPTIONS 요청을 받은 서버는 **Response Header에 서버가 허용할 옵션을 설정하여 브라우저에게 전달**한다.

예를 들어 응답 헤더에 **`Access-Control-Allow-Origin`** 항목을 추가하여 허용할 도메인을 지정할 수 있는데, 설정하게 되면 개발자 도구에서 아래와 같이 확인할 수 있다.

```null
Access-Control-Allow-Origin: https://example.com
```

브라우저는 서버가 보낸 Response 정보를 이용하여 허용되지 않은 요청인 경우 405 Method Not Allowed 에러를 발생시키고, 실제 페이지의 요청(본 요청)은 서버로 전송하지 않고, 반대로 **허용된 요청인 경우 본 요청을 보낸다.**



![[Pasted image 20241022090837.png]]


preflight 시나리오의 플로우를 자세히 살펴보면 아래와 같다.


![[Pasted image 20241022090848.png]]




1. 브라우저는 위 XMLHttpRequest가 Cross-Origin 요청인 것을 판단하여 아래와 같이 요청에 "Origin: [https://foo.example](https://foo.example/)" 헤더를 추가한다. 또한 브라우저는 **POST 방식**이며, **Content-Type이 application/x-www-form-urlencoded, multipart/form-data, text/plain에 포함되지 않기** 때문에 Prefight Request 방식으로 보내야 한다는 것을 알고 있다. 그래서 브라우저는 요청에 아래와 같이 헤더 정보를 추가하여 외부 서버로 Preflight Request(예비 요청)을 보낸다.
    
2. 서버는 **이 preflight Request(예비 요청)에 대한 응답**으로 현재 자신이 어떤 것들을 허용하고 있는지에 대한 **정보를 response header에 담아서 브라우저에게 다시 보내주게 된다.**  
    

```null
- Access-Control-Allow-Origin: https://foo.example
- Access-Control-Allow-Methods: POST, GET, OPTIONS  
- Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
- Access-Control-Max-Age: 86400
```

위 헤더들은 다음과 같은 뜻이다.  
Access-Control-Allow-Origin : 허가된 Origin  
Access-Control-Allow-Methods : 허가된 메소드  
Access-Control-Allow-Headers : 허가된 헤더  
Access-Control-Max-Age : 응답 캐시가 유효 시간

3. 응답으로 받은 response header의 정보를 통해서 브라우저는 본 요청을 외부 서버로 보낼지 말지를 판단하게 된다. 위 예시에서 표시된 정보는 **'해당 API가 Cross-Origin에 대해서 POST, GET, OPTIONS와 커스텀 헤더인 X-PINGOTHER 그리고 Content-Type을 허용한다'**는 의미이다. 이에 해당되는 것들은 안전하다고 판단하여 CORS 위반으로 간주하지 않고, 브라우저는 POST인 본 요청을 브라우저는 외부 서버로 보낸다.

  

++ 참고로 Preflight Request 방식은 많은 리소스를 잡아 먹는다. 그렇기 때문에 서버에서 **"Access-Control-Max-Age" 헤더 정보**를 통해서 Preflight Request 를 캐싱함으로써 그 효율을 높힐 수 있다.

[[Web] CORS 동작 방식과 해결 방법](https://ingg.dev/cors/#cors_solution)

  

++ 응답 헤더와 관련해서는 다음 정보들을 참고해보자.  
[(HTTP) 알아둬야 할 HTTP 응답 헤더](https://www.zerocho.com/category/HTTP/post/5b4c4e3efc5052001b4f519b)  
[[프론트엔드] CORS의 유래 및 특징, 활용](https://velog.io/@kansun12/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C-CORS)  
[CORS](https://koguri.tistory.com/65)

  
  

- **주의할 점!**

예비 요청에 대한 응답에서 에러가 발생하지 않고 정상적으로 **`200`**이 떨어졌는데, 콘솔 창에는 빨갛게 에러가 표시되는 경우가 있다. 이는 CORS 정책 위반으로 인한 에러는 예비 요청의 성공 여부와 별 상관이 없다. 브라우저가 CORS 정책 위반 여부를 판단하는 시점은 예비 요청에 대한 응답을 받은 이후이기 때문이다.

물론 예비 요청 자체가 실패해도 똑같이 CORS 정책 위반으로 처리될 수도 있지만, 중요한 것은 예비 요청의 성공/실패 여부가 아니라 “응답 헤더에 유효한 **`Access-Control-Allow-Origin`** 값이 존재하는가”이다. 만약 예비 요청이 실패해서 **`200`**이 아닌 상태 코드가 내려오더라도 헤더에 저 값이 제대로 들어가있다면 CORS 정책 위반이 아니라는 의미이다.

  

> 참고 :  
> 1. **Access-Control-Allow-Origin** 허용한 상태


![[Pasted image 20241022090900.png]]


2. **Access-Control-Allow-Origin** 허용하지 않은 상태


![[Pasted image 20241022090905.png]]


> > 두 캡처 사진에서 볼 수 있듯이 응답 상태는 200이지만, **Access-Control-Allow-Origin**에서 차이가 있는 것을 확인 가능.
> 
> 참고로 "Access-Control-Allow-Origin: *" 이란 것은 어떠한 Origin 이든 허용한다는 뜻이며, 특정 Origin만 허용하고 싶다면, 서버에서 응답헤더에 "Access-Control-Allow-Origin: [https://foo.example](https://foo.example/)" 로 값을 설정해 주면 된다.

  

- **근데 왜 Postman은 CORS에 안 걸리지?**  
    preflight request 조건에 해당되는 경우지만, preflight request를 보내지 않는 경우도 있다. **우선 브라우저를 쓰지 않으면 보내지 않는데, 앞서 말했듯이 origin이 다른지 판단하는 것은 브라우저 스펙이기 때문.**  
    그래서 Postman과 같은 기능을 사용하면 json 형태든, POST로 보내든 CORS 문제가 발생하지 않는다.

  
  
  

### 3. Credential Request

3번째 시나리오는 헤더에 인증과 관련된 정보(쿠키, 토큰 등)를 담아서 보내는 Credential Request (인증된 요청)을 사용하는 방법이다.  
CORS의 기본적인 방식이라기 보다는 다른 출처 간 통신에서 좀 더 보안을 강화하고 싶을 때 사용하는 방법이다.

예를 들어, 자바스크립트의 fetch API를 사용하거나 Axios, Ajax 등을 사용할 때 서버로 쿠키를 함께 전송해야 하는 경우가 있는데, 요청에 쿠키가 담기게 되면 Credentialed Request 허용이 되어 있어야 한다.

즉, **인증과 관련된 정보를 담을 수 있게 해주는 옵션 'credentials'**를 줘야하는데, 이 때 서버 쪽에서 응답 헤더에 Access-Control-Allow-Credentials: true를 보내주지 않는다면 브라우저에서 응답을 받는 것을 거부하게 된다.

이 옵션에는 총 3가지의 값을 사용할 수 있으며, 각 값들이 가지는 의미는 다음과 같다.

|옵션 값|설명|
|---|---|
|same-origin (기본값)|같은 출처 간 요청에만 인증 정보를 담을 수 있다|
|include|모든 요청에 인증 정보를 담을 수 있다|
|omit|모든 요청에 인증 정보를 담지 않는다|

만약 **`same-origin`**이나 **`include`**와 같은 옵션을 사용하여 리소스 요청에 인증 정보가 포함된다면, 이제 브라우저는 다른 출처의 리소스를 요청할 때 단순히 **`Access-Control-Allow-Origin`**만 확인하는 것이 아니라 좀 더 빡빡한 검사 조건을 추가하게 된다.

요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 룰에 다음 두 가지를 추가하게 된다.

1. **`Access-Control-Allow-Origin`**에는 **`*` (와일드카드)**를 사용할 수 없으며, 명시적인 URL이어야한다.  
    ([https://foo.com과](https://foo.xn--com-rg8l/) 같이 구체적인 origin을 지정해주어야 한다.)
    
2. 응답 헤더에는 반드시 **`Access-Control-Allow-Credentials: true`**가 존재해야한다.
    

  
  

## CORS 에러 해결방법

### 1. Access-Control-Allow-Origin 세팅하기

> CORS 정책 위반으로 인한 문제를 해결하는 가장 대표적인 방법은, 그냥 정석대로 서버에서 **`Access-Control-Allow-Origin`** 헤더에 알맞은 값을 세팅해주는 것이다.  
>   
> 이때 와일드카드인 **`*`**을 사용하여 이 헤더를 세팅하게 되면 모든 출처에서 오는 요청을 받아먹겠다는 의미이므로 당장은 편할 수 있겠지만, 바꿔서 생각하면 정체도 모르는 이상한 출처에서 오는 요청까지 모두 받아먹겠다는 오픈 마인드와 다를 것 없으므로 보안적으로 심각한 이슈가 발생할 수도 있다.  
>   
> 그러니 가급적이면 귀찮더라도 **`Access-Control-Allow-Origin: https://evan.github.io`**와 같이 출처를 명시해주도록 하자.

  

*_- 하지만 서버의 목적에 따라 Access-Control-Allow-Origin에 와일드카드(_)를 줘야할 때가 있다!

일단 credential은 와일드카드가 아니라 명확한 url로 줘야하는 거 알겠고,  
가급적이면 명시적으로 url을 줘야하는 거는 알겠는데,  
그럼 와일드카드로 줘야하는 경우는 없을까?

답은 **'서버의 목적에 따라 와일드카드로 줘야할 때가 있다'**이다.

예를 들면 일반 서버와 (널리 쓰이는) API 서버는 허용 origin 범위를 다르게 해야할 것이다.  
목적에 따라 클라이언트 사이드의 보안 수준을 달리하는 것이다.

API 서버들은 SOP나 CORS에 관계 없이 접근이 가능해야 하고, 사용자의 쿠키 등 중요한 데이터를 보관하지 않는다.  
이런 경우에는 Access-Control-Allow-Origin: * 와 같은 헤더를 적용하여 클라이언트 사이드에서 낮은 수준의 보안을 유지해도 괜찮다.

일반 서버들은 자신의 사이트를 이용하는 사용자의 정보가 중요하기 때문에 Access-Control-Allow-Origin 헤더를 자신의 사이트로 한정하는 등, 엄격한 기준을 적용하는 보안이 필요하다.

  
  

### 2. Webpack Dev Server로 리버스 프록싱하기

> 사실 CORS를 가장 많이 마주치는 환경은 바로 로컬에서 프론트엔드 어플리케이션을 개발하는 경우라고 해도 과언이 아니다. 백엔드에는 이미 **`Access-Control-Allow-Origin`** 헤더가 세팅되어있겠지만, 이 중요한 헤더에다가 **`http://localhost:3000`** 같은 범용적인 출처를 넣어주는 경우는 드물기 때문이다.  
>   
> 프론트엔드 개발자는 대부분 웹팩과 **`webpack-dev-server`**를 사용하여 자신의 머신에 개발 환경을 구축하게 되는데, 이 라이브러리가 제공하는 프록시 기능을 사용하면 아주 편하게 CORS 정책을 우회할 수 있다.  
>   
> 이렇게 설정을 해놓으면 로컬 환경에서 **`/api`**로 시작하는 URL로 보내는 요청에 대해 브라우저는 **`localhost:8000/api`**로 요청을 보낸 것으로 알고 있지만, 사실 뒤에서 웹팩이 **`https://api.evan.com`**으로 요청을 프록싱해주기 때문에 마치 CORS 정책을 지킨 것처럼 브라우저를 속이면서도 우리는 원하는 서버와 자유롭게 통신을 할 수 있다. 즉, 프록싱을 통해 CORS 정책을 우회할 수 있는 것이다.  

하지만 이 방법은 로컬 개발 환경에서만 통하는 방법인데다가, 근본적인 문제 해결 방법이 아니기 때문에,  
결국 운영 환경에서 CORS 정책 위반 문제를 해결하기 위해서는 백엔드 개발자가 서버 어플리케이션의 응답 헤더에 올바른 Access-Control-Allow-Origin이 내려오도록 세팅해줄 수밖에 없다.





---
출처 - https://velog.io/@effirin/CORS%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80