
![[Pasted image 20240903152158.png]]




![[Pasted image 20240903152204.png]]




```
[https:://test.cafe24.com/api/v2/oauth/authorize?response_type=code&client_id=K4zSI7doDOVpvLhsjO4blBB&state=ssasd=&redirect_uri=vlutestid.cafe24.com/&scope=mall.read_application,mall.write_application](https://vlutestid.cafe24.com/api/v2/oauth/authorize?response_type=code&client_id=K4zSI7oDOVpvLhsjO4blBB&state=ssasd=&redirect_uri=https://vlutestid.cafe24.com/&scope=mall.read_application,mall.write_application)

```


![[Pasted image 20240903152505.png]]

내가 입력한 clientId, redirectUri -> 내 실제 주소 입력 가능

실제로 입력한 모습 
```
https://tmdals23222.cafe24.com/api/v2/oauth/authorize?response_type=code&client_id=Ze7D6V5nQS2PPoXlfE6yGB&state=ssasd%3D&redirect_uri=https://tmdals23222.cafe24.com/&scope=mall.read_application%2Cmall.write_application&shop_no=1&callback_uri=
```

발급받은 코드는 1분뒤 폐기

```
https://tmdals23222.cafe24.com/?code=anCHXp9wEJTqccl2DToMWD&state=ssasd=
```

![[Pasted image 20240903152624.png]]

PostMan 접속후 회원 가입

https://www.postman.com/

![[Cafe24 PostMan1.png]]

cafe24의 Run in Postman 클릭

![[Cafe24 PostMan2.png]]

앱으로 할지 웹으로 할지는 선택 앱이 없을 경우 밑에 get the app 클릭후 앱 다운로드 

![[Cafe24 PostMan3.png]]


웹 클릭시 웹 페이지로 넘어가며 import 할꺼냐 뜨고  app 클릭시 app으로  어떤 workspace에 import 할지 나옴
내가 원하는 workspace 클릭
![[Cafe24 PostMan4.png]]

import시 이렇게 api가 들어옴 

![[Cafe24 PostMan5.png]]

카페24 developers 에서 카페24 api 환경설정 파일 클릭시 파일 다운로드

![[Cafe24 PostMan6.png]]


파일 다운후 압축 해제 -> Environments 메뉴로 이동 후 Cafe24 API Environment 파일을 업로드 


![[Cafe24 PostMan7.png]]

밑에 항목 입력

| 항목                  | 설명                                                                                                                     |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| mallid              | 테스트를 적용할 쇼핑몰 아이디  <br><br>카페24 회원가입 시 자동 생성된 쇼핑몰을 활용하실 수 있습니다.  <br>쇼핑몰 아이디는 카페24 아이디와 동일합니다.                          |
| redirect_uri        | [개발자 어드민 > 앱 기본정보]에 등록되어 있는 Redirect URI를 정확하게 입력하세요.                                                                  |
| client_id           | 앱 생성 시 발급 받은 클라이언트 아이디                                                                                                 |
| client_secret       | 앱 생성 시 발급 받은 클라이언트 시크릿                                                                                                 |
| version             | [개발자 어드민 > 앱 관리]에 표시되어있는 API Version을 입력하세요. 예) 2021-09-01                                                             |
| authorization_basic | client_id와 client_secret 키를 client_id:client_secret 형식으로 base64 인코딩하여 입력합니다.  <br>![[Cafe24 PostMan8.png]] |


![[Cafe24 PostMan9.png]]

실제 입력

![[Cafe24 PostMan10.png]]



client ID, client Secret Key, service Key는 app관리에 들어가면 있음

![[Cafe24 PostMan11.png]]


**Note!**

환경 설정이 완료된 후 반드시 POSTMAN 화면 우측 상단에서 'No Environment'대신 'Cafe24 API Environment'를 적용하세요.

![[Cafe24 PostMan12.png]]

![[Cafe24 PostMan13.png]]

## 액세스 토큰 발급

-  cafe24 API 콜렉션 중 'Authorize Access Token' 항목을 선택
- 2. Authorization 탭으로 이동하여, TYPE을 OAuth2.0으로 선택한 후 아래의 값들을 입력하고 화면 하단의 Get New Access Token 버튼을 클릭

![[Cafe24 PostMan14.png]]


아래 항목 입력

| Key                   | 필수여부 | 기본값                                                     | 설명                                                                                                                                                                                                                                                 |
| --------------------- | ---- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Token Name            | 필수   | access_token                                            | 발급받을 액세스 토큰의 이름입니다. 원하는 이름을 자유롭게 입력하세요.                                                                                                                                                                                                            |
| Grant Type            | 필수   | Authorization Code                                      | 요청 타입을 의미합니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                                       |
| Callback URL          | 필수   | {{redirect_uri}}                                        | 환경설정에서 입력한 Redirect URI가 적용됩니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                     |
| Auth URL              | 필수   | https://{{mallid}}.cafe24api.com/api/v2/oauth/authorize | 인증 코드 요청을 처리하는 URI입니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                              |
| Access Token URL      | 필수   | https://{{mallid}}.cafe24api.com/api/v2/oauth/token     | 액세스 코드 요청을 처리하는 URI입니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                             |
| Client ID             | 필수   | {{client_id}}                                           | 환경설정에서 입력한 클라이언트 아이디가 적용됩니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                        |
| Client Secret         | 필수   | {{client_secret}}                                       | 환경설정에서 입력한 클라이언트 시크릿이 적용됩니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                        |
| Scope                 | 필수   | -                                                       | 앱에서 사용하는 API를 기준으로 권한 정보를 입력하세요.<br>Scope는 개발자 어드민 > 앱 기본정보 등록 화면에서 입력한 권한관리 항목과 동일해야 합니다.  <br><br>권한이 여러 개인 경우에는 콤마(,)로 연결하여 입력하세요.  <br>예) 앱 읽기 + 쓰기권한, 상품분류 읽기권한을 포함하는 경우  <br>mall.read_application,mall.write_application,mall.read_category |
| State                 | 권장   | -                                                       | 사이트 간 요청 위조(CSRF) 공격을 방지하기 위한 값입니다. Client-side에서 임의의 문자열을 생성하여 입력하세요.                                                                                                                                                                             |
| Client Authentication | 필수   | Send a Basic Auth header                                | 인증 정보를 전달할 위치를 의미합니다. 기본값 그대로 사용하세요.                                                                                                                                                                                                               |


![[Cafe24 PostMan15.png]]

실제 적용 내용 


![[Cafe24 PostMan16.png]]

여기서 
![[Cafe24 PostMan17.png]]
이 부분은 

![[Cafe24 PostMan18.png]]

여기와 똑같아야함 

저기서  파란 버튼 클릭시  이렇게 나오는데 여기서 맞는 권한 가져다가 쓰기

![[Cafe24 PostMan19.png]]


3. Get New Access Token 버튼을 클릭하면 쇼핑몰 관리자 로그인 창이 표시. 대표관리자 계정으로 로그인


![[Cafe24 PostMan20.png]]



로그인에 성공하면 인증 정보를 확인

![[Cafe24 PostMan21.png]]

> Note
> 액세스 토큰을 복사하여 메모장 등에 임시저장하세요.


- 5. 문서 상단의 환경설정 방법을 참고하여 'Manage Environment' 화면으로 이동.
- 6. 'Cafe24 API Environment'에 아래 항목을 추가하고 Save 버튼을 클릭하세요. (CURRENT VALUE에 동일한 값을 입력)

| 항목           | 설명                       |
| ------------ | ------------------------ |
| access_token | 4번에서 임시저장한 액세스 토큰 값을 입력. |

- 이제 액세스 토큰을 이용하여 어드민 API를 호출할 수 있습니다. (Authorization 탭 > 인증 타입을 OAuth 2.0으로 선택)  
    
    **Caution!**
    
    액세스 토큰의 유효기간은 2시간.


## 실제 발급 

아까 인증 받은 코드를 Body 부분에 추가

![[Cafe24 PostMan16.png]]

![[Pasted image 20240903162713.png]]
추가후 나머지 부분은 동일 그리고 send로 값 가져오기
성공시 밑에 데이터 표출

## 액세스 토큰 재발급

- 1. cafe24 API 콜렉션 중 'Authorize Access Token via Refresh Token' 항목을 선택.
- 2. Authorization 탭으로 이동 이동하여 Type을 OAuth2.0으로 선택한 후 화면 하단의 Get New Access Token 버튼을 클릭.








---
자료 - https://algoisanswer.tistory.com/18
출처 - 