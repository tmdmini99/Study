
## **POSTMAN 이란?**

API 개발을 보다 빠르고 쉽게 구현 할 수 있도록 도와 줌.

개발된 API를 테스트하여 문서화 또는 공유 할 수 있도록 해주는 플랫폼.

## POSTMAN을 왜 사용할까?

URL을 통해 테스트는 한계가 있음.

실제 개발 경우, 응답 요청과 응답 받아 화면에 출력하는 등의 작업이 길어짐

Authorization이나 Header, Body를 수정하는건 더더욱 제한이 많음.

하지만 포스트맨은 해당 작업을 할 수 있도록 인터페이스를 구축해둔 툴이기 때문에 쉽게 사용가능

OS에 상관없고 어디서나 사용가능.

계정을 보유하고 있다면, 내가 요청한 Request 히스토리, 테스트한 환경 그대로 저장되어

언제 어디서나 내가 작업했던 환경이 구축됨.

---

## POSTMAN  사용 방법

### **1.** POSTMAN 설치

**[https://www.postman.com/](https://www.postman.com/)**


### 2. 다운로드 클릭


![[PostMan1.png]]


### 2. 회원가입

![[PostMan2.png]]


### 3. 메인 화면  →  Workspaces  → New  Workspaces



![[PostMan3.png]]


### 4.  Create New Workspce

![[PostMan4.png]]


1) Name 입력

2) visibility 설정

3) crate Workspce and team 클릭

### 5.  Workspces


![[PostMan5.png]]

### 5.  테스트 해보기!!


![[PostMan6.png]]


1) GET,POST,PUT...  등등 선택

2) 테스트할 URL 입력

3) Send 클릭!

4) Body 에서 보여줄 데이터 형식이 Json,XMl HTML... 등등으로 선택


[[Cafe24 Authorization Code &PostMan & Access Token]]에서 확인 가능 api 적용


|                                                  |                                                                                                                                                                                                                                                                                                                                    |                                  |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| **SNIPPETS**                                     | **Tests Code**                                                                                                                                                                                                                                                                                                                     | **설명**                           |
| Get an environment variable                      | pm.environment.get("variable_key");                                                                                                                                                                                                                                                                                                | Postman 전체에서 사용 가능한 파라미터 값 얻음    |
| Get a global variable                            | pm.globals.get("variable_key");                                                                                                                                                                                                                                                                                                    | 전역 파라미터 값 얻기                     |
| Get a variable                                   | pm.variables.get("variable_key");                                                                                                                                                                                                                                                                                                  | Collection 안에서 사용 가능한 파라미터 값 얻기  |
| Set an environment variable                      | pm.environment.set("variable_key", "variable_value");                                                                                                                                                                                                                                                                              | Postman 전체에서 사용 가능한 파라미터 값 설정    |
| Set a global variable                            | pm.globals.set("variable_key", "variable_value");                                                                                                                                                                                                                                                                                  | 전역 파라미터 값 설정                     |
| Clear an environment variable                    | pm.environment.unset("variable_key");                                                                                                                                                                                                                                                                                              | Postman 전체에서 사용 가능한 파라미터 삭제      |
| Clear a global variable                          | pm.globals.unset("variable_key");                                                                                                                                                                                                                                                                                                  | 전역 파라미터 삭제                       |
| Send a request                                   | pm.sendRequest("https://postman-echo.com/get", function (err, response) {  <br>    console.log(response.json());  <br>});                                                                                                                                                                                                          | API 호출                           |
| Status code: Coet is 200                         | pm.test("Status code is 200", function () {  <br>    pm.response.to.have.status(200);  <br>});                                                                                                                                                                                                                                     | Http 코드가 200인지 확인                |
| Response body: Contains string                   | p[m.test(](http://m.test(/)"Body matches string", function () {  <br>  <br>p[m.expect(pm.response.text()).to.include(](http://m.expect(pm.response.text()).to.include(/)"string_you_want_to_search");  <br>});                                                                                                                     | 응답 결과 중 원하는 단어 포함 여부 확인          |
| Response body: Is equal to a string              | p[m.test(](http://m.test(/)"Body is correct", function ()  <br>  <br>{pm.response.to.have.body("response_body_string");  <br>});                                                                                                                                                                                                   | 응답 결과가 원하는 결과인지 확인               |
| Response headers: Content-Type header check      | pm.test("Content-Type is present", function () {  <br>    pm.response.to.have.header("Content-Type");  <br>});                                                                                                                                                                                                                     | 응답 header 중 원하는 타입이 있는지 확인함.     |
| Response time is less than 200ms                 | p[m.test(](http://m.test(/)"Response time is less than 200ms", function () {  <br>  <br>  <br>p[m.expect(pm.response.responseTime).to.be.below(200);](http://m.expect(pm.response.responsetime).to.be.below(200);/)  <br>});                                                                                                       | 응답 시간이 200ms이하인지 확인              |
| Status code: Successful POST request             | pm.test("Successful POST request", function () {  <br>  <br>pm.expect(pm.response.code).to.be.oneOf([201,202]);  <br>});                                                                                                                                                                                                           | http 코드가 201, 202 중 하나인지 확인      |
| Status code: Code name has string                | pm.test("Status code name has string", function () {  <br>    pm.response.to.have.status("Created");  <br>});                                                                                                                                                                                                                      | 응답 코드에 원하는 단어 포함 여부 확인           |
| Response body: Convert XML body to a JSON Object | var jsonObject = xml2Json(responseBody);                                                                                                                                                                                                                                                                                           | XML을 JSON으로 변환                   |
| Use Tiny Validator for JSON data                 | var schema = {  <br>  "items": {  <br>    "type": "boolean"  <br>  }  <br>};  <br>   <br>var data1 = [true, false];  <br>var data2 = [true, 123];  <br>   <br>pm.test('Schema is valid', function() {  <br>  pm.expect(tv4.validate(data1, schema)).to.be.true;  <br>  pm.expect(tv4.validate(data2, schema)).to.be.true;  <br>}); | 응답 값인 JSON 데이터데 원하는 Schema 여부 확인 |
|                                                  |                                                                                                                                                                                                                                                                                                                                    |                                  |




---
출처 - https://dev-cini.tistory.com/7

https://blog.naver.com/wisestone2007/221393509035