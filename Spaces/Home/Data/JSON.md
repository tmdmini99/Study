# json이란?
•JavaScript Object Notation라는 의미의 축약어로 데이터를 저장하거나 전송할 때 많이 사용되는 경량의 DATA 교환 형식

•Javascript에서 객체를 만들 때 사용하는 표현식을 의미한다.

•JSON 표현식은 사람과 기계 모두 이해하기 쉬우며 용량이 작아서, 최근에는 JSON이 XML을 대체해서 데이터 전송 등에 많이 사용한다.

•JSON은 데이터 포맷일 뿐이며 어떠한 통신 방법도, 프로그래밍 문법도 아닌 단순히 데이터를 표시하는 표현 방법일 뿐이다.

• 키와 값을 괄호와 세미콜론과 같이 간단한 기호로 구성하여 표현할 수 있고, 언어나 운영체제에 구애받지 않기 때문에 많이 사용됨

• 웹/앱 환경에서 Rest API를 사용하여, 서버와 클라이언트 사이에 데이터를 주고 받을때 많이 사용

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

https://amor-dev.tistory.com/13