# json이란?
•JavaScript Object Notation라는 의미의 축약어로 데이터를 저장하거나 전송할 때 많이 사용되는 경량의 DATA 교환 형식

•Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.

•JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.

•JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다.

• 키와 값을 괄호와 세미콜론과 같이 간단한 기호로 구성하여 표현할 수 있고, 언어나 운영체제에 구애받지 않기 때문에 많이 사용됨

• 웹/앱 환경에서 Rest API를 사용하여, 서버와 클라이언트 사이에 데이터를 주고 받을때 많이 사용

-   
    키와 문자열 값은 큰 따옴표로 둘러싸야 함  
    JSON에서는 키와 문자열 값은 큰 따옴표로 둘러싸야 한다. 작은 따옴표나 백틱은 사용할 수 없다.
    
- 문자열은 이스케이프 가능  
    문자열 값 내에서 특수 문자를 이스케이프하여 표현할 수 있다.
    
- 중첩 구조  
    JSON 객체 내에서 다른 JSON 객체를 중첩하여 사용할 수 있으며, 배열을 사용하여 여러 값을 저장할 수 있다.
    
- 언어 독립적  
    JSON은 언어 독립적인 형식이므로 다양한 프로그래밍 언어 간에 데이터를 교환하는 데 사용할 수 있다.
    
- 웹 API 통신: 웹 애플리케이션에서 서버와의 통신에 JSON을 주로 사용한다. 서버에서 클라이언트로 데이터를 보낼 때나 클라이언트에서 서버로 데이터를 전송할 때 주로 JSON 형식을 사용한다.
    
- JSON.stringify와 JSON.parse  
    JavaScript에서 JSON을 다룰 때 JSON.stringify 함수를 사용하여 JavaScript 객체를 JSON 문자열로 직렬화하고, JSON.parse 함수를 사용하여 JSON 문자열을 JavaScript 객체로 역직렬화한다.
    
- JSON Schema  
    JSON 데이터의 유효성 검사 및 문서화를 위한 JSON Schema라는 표준이 있다.

#### Object

Object는 { }(curly brace)로 감싸여 있는 것을 말합니다. 예를들어 아래 JSON 코드는 1개의 Object가 있고 그 Object는 title, url, draft, star라는 4개의 key와 그에 해당하는 value를 갖고 있습니다.

```json
{
  "title": "how to get stroage size",
  "url": "https://codechacha.com/ko/get-free-and-total-size-of-volumes-in-android/",
  "draft": false,
  "star": 10
}
```
#### Key-Value

Key-Value는 위의 title을 예로 들면, "title"이 key이고, :다음에 있는 "how to get stroage size"이 value입니다. key는 꼭 "..." 처럼 콜론으로 감싸져야 합니다. value는 String 타입인 경우 "..."으로 표현하고 Boolean이나 Integer는 그냥 쓰면 됩니다.

```json
"title": "how to get stroage size",
```


#### Array

Array는 \[ ](square bracket)으로 감싸여 있는 것을 말합니다.

```json
{
  "posts": [
      {
          "title": "how to get stroage size",
          "url": "https://codechacha.com/ko/get-free-and-total-size-of-volumes-in-android/",
          "draft": false
      },
      {
            "title": "Android Q, Scoped Storage",
            "url": "https://codechacha.com/ko/android-q-scoped-storage/",
            "draft": false
      },
      {
            "title": "How to parse JSON in android",
            "url": "https://codechacha.com/ko/how-to-parse-json-in-android/",
            "draft": true
      }
  ]
}
```



```JSON
{
  "people": [
    {
      "name": "Gildong",
      "lastName": "Hong"
    },
    {
      "name": "Fox",
      "lastName": "Im"
    },
    {
      "name": "Yushine",
      "lastName": "Kim"
    } 
  ]
}
```


## Json과 XML의 공통점
- 둘 다 데이터를 저장하고 전달하기 위해 고안되었습니다.
- 둘 다 기계뿐만 아니라 사람도 쉽게 읽을 수 있습니다.
- 둘 다 계층적인 데이터 구조를 가집니다.
- 둘 다 다양한 프로그래밍 언어에 의해 파싱될 수 있습니다.

## Json과 XML의 차이점
- JSON은 종료 태그를 사용하지 않습니다.
- JSON의 구문이 XML의 구문보다 더 짧습니다.
- JSON 데이터가 XML 데이터보다 더 빨리 읽고 쓸 수 있습니다.
- XML은 배열을 사용할 수 없지만, JSON은 배열을 사용할 수 있습니다.


---

# JsonObject/JsonArray
•JSONObject란 JSON 형식의 String을 처리하도록 도와주는 Java 라이브러리이다.

•JSONObjent 는 { } 로 JSONArray는 [ ] 로 묶어 나뉜다.

•JsonArray안엔 json 데이터가 배열로 들어가는걸 뜻하는데, JsonArray에 jsonObject 객체를 저장하거나, Json 데이터를 배열형태로 저장하는게 가능하다. 또한 JsonArray에 있는 데이터를 순서대로 꺼내서 출력도 가능하다.





---
# GeoJson

형식
```geojson
{
    "type": "Polygon",
    "coordinates": [
        [[30, 10], [40, 40], [20, 40], [10, 20], [30, 10]]
    ]
}
```


위도 경도  반대로 되어있음

원래는 
위도     경도 
geojson
경도 위도



@JsonNaming, @JsonProperty의 사용법

```json
{ "my_name": "kevin", "my_age": 20, "my_country": "korea" }
```



---

https://amor-dev.tistory.com/13


https://akku-dev.tistory.com/54  json 이쁘게 출력

https://mondaymonday2.tistory.com/601

