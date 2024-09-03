## 액세스 토큰 Request

- 액세스 토큰은 RESTful API 형태로 요청하실 수 있습니다.

|Method|Resource URI|Format|
|---|---|---|
|POST|https://{mall_id}.cafe24api.com/api/v2/oauth/token|x-www-form-urlencoded|

### 액세스 토큰 Request 형식

```
curl -X POST
'https://{mall_id}.cafe24api.com/api/v2/oauth/token'
-H 'Authorization: Basic {base64_encode({client_id}:{client_Secret})}'
-H 'Content-Type: application/x-www-form-urlencoded'
-d 'grant_type=authorization_code'
-d 'code={authorization_code}'
-d 'redirect_uri={redirect_uri}'
```

샘플 보기

```
curl -X POST
'https://samplemall.cafe24api.com/api/v2/oauth/token'
-H 'Authorization: Basic sampleRCTjdPVk5uQjNGMHM3UzFNRDpFaEZnM0xYak1KR21BZWV5MUliaXhI'
-H 'Content-Type: application/x-www-form-urlencoded'
-d 'grant_type=authorization_code'
-d 'code=sampleXeWS9W5q08ybH1XHS'
-d 'redirect_uri=https://sample-app.com/oauth/redirect'
```

**Note!**

### Header의 {base64_encode({client_id}:{client_Secret})} 부분은 {클라이언트 아이디}:{클라이언트 시크릿}을 Base64 방식으로 인코딩하여 입력하세요.

| 파라미터          | 필수여부                | 설명                                                            |
| ------------- | ------------------- | ------------------------------------------------------------- |
| grant_type    | 필수                  | 인증 형식입니다. "authorization_code"로 입력하세요.                        |
| code          | 필수                  | 이전 단계에서 획득한 인증 코드(Authorization Code) 값을 입력하세요.               |
| redirect_uri  | 필수                  | 개발자센터에서 입력한 redirect_uri중 액세스토큰을 처리할수 있는 redirect_uri를 입력하세요. |
| code_verifier | 필수  <br>(PKCE 활성화시) | Code Challenge 생성 시 사용한 code_verifier 문자열                     |

## 액세스 토큰 Response

- Request에 성공하면 다음과 같은 응답을 확인하실 수 있습니다.

### 액세스 토큰 Response 형식

```json
HTTP/1.1 200 OK
{
  "access_token": "{access_token}",
  "expires_at": "{expiration_date_of_access_token}",
  "refresh_token": "{refresh_token}",
  "refresh_token_expires_at": "{expiration_date_of_refresh_token}",
  "client_id": "{client_id}",
  "mall_id": "{mall_id}",
  "user_id": "{user_id}",
  "scopes": [
    "{scopes_1}",
    "{scopes_2}"
  ],
  "issued_at": "{issued_date}"
}
```

샘플 보기

```json
HTTP/1.1 200 OK
{
  "access_token": "sample9jIRUGHE5CBOiKRGC",
  "expires_at": "2018-11-07T20:12:25.916",
  "refresh_token": "sample80BQWWCJEiwTHWCrU",
  "refresh_token_expires_at": "2018-11-21T18:12:25.918",
  "client_id": "sample7eBNEqSfkd7I8hoA",
  "mall_id": "samplemall",
  "user_id": "jonhdoe123",
  "scopes": [
    "mall.read_application",
    "mall.write_application",
    "mall.read_category"
  ],
  "issued_at": "2018-11-07T18:12:25.918"
}
```

### 파라미터 정보

| 파라미터                     | 설명                                                            |
| ------------------------ | ------------------------------------------------------------- |
| access_token             | API 호출시 필요한 액세스 토큰                                            |
| expires_at               | 액세스 토큰 만료 일시/ 발급으로부터 2시간 후 만료됩니다.                             |
| refresh_token            | 액세스 토큰을 재발급 할 수 있는 토큰  <br>리프레시 토큰에 대한 자세한 내용은 다음 문서를 참조하세요.  |
| refresh_token_expires_at | 리프레시 토큰 만료 일시/ 발급으로부터 14일 후 만료됩니다.                            |
| client_id                | 클라이언트 아이디                                                     |
| mall_id                  | 해당 쇼핑몰의 아이디                                                   |
| user_id                  | 쇼핑몰 사용자(운영자) 아이디                                              |
| scopes                   | 사용 동의된 Scope 정보                                               |
| issued_at                | 액세스 토큰 발급 일시                                                  |

**Note!**

Date 관련 데이터는 ISO_8601 포맷(yyyy-mm-ddThh:mm:ss.sssZ)을 사용합니다.

## 에러 코드 안내

- Request에 실패하면 에러 코드를 반환합니다.

### 에러 코드 Response 형식

```json
HTTP/1.1 401 Unauthorized
{
    "error":"{error}",
    "error_description":"{error_description}"
}
```

**샘플 보기**열기

### 파라미터 정보

| 파라미터              | 설명                                               |
| ----------------- | ------------------------------------------------ |
| error             | OAuth 2.0 Framework Standard 5.2에 정의된 에러 코드값입니다. |
| error_description | 오류에 대한 자세한 설명입니다.                                |

### 에러 코드 안내

- 에러 코드 별 설명 및 그에 따른 대응 방법을 안내합니다.

| 에러 코드                     | HTTP Status                                      | 설명                                              | 대응 방법                                          |
| ------------------------- | ------------------------------------------------ | ----------------------------------------------- | ---------------------------------------------- |
| invalid_request           | 400                                              | redirect_uri/ authorization_code 값을 누락한 경우      | 누락된 값을 확인하여 입력하세요.                             |
| invalid_grant             | 400                                              | 잘못된 authorization_code를 사용한 경우                  | 올바른 인증 코드인지 확인하세요.                             |
| 400                       | 만료된 authorization_code를 사용한 경우                   | 새로운 인증 코드를 요청하세요.                               |                                                |
| unsupported_response_type | 400                                              | grant_type 값이 누락되거나 "authorization_code"가 아닐 경우 | 'grant_type=authorization_code' 로 되어있는지 확인하세요. |
| invalid_scope             | 401                                              | client_id/ client_secret 값을 누락한 경우              | 헤더(Header)의 Authorization 값이 누락되었는지 확인하세요.     |
| 401                       | 잘못된 client_id/ client_secret/ redirect_uri 값인 경우 | 입력한 값이 개발자 어드민에 앱 기본정보와 동일한지 확인하세요.             |                                                |



## 리프레시 토큰

- 액세스 토큰(Access Token)의 유효기간은 2시간이므로, 지속적으로 사용하려면 유효기간이 만료되기 전에 재발급해야 합니다.
- 때문에 인증 서버에서는 액세스 토큰 재발급 용도의 리프레시 토큰(Refresh Token)을 함께 제공합니다.

**Note!**

액세스 토큰은 보호된 데이터에 대한 접근 권한을 증명하는 값이므로 외부에 노출되는 경우 치명적인 보안 이슈가 발생하게 됩니다.  
때문에 만약 탈취되더라도, 해커에 의해 남용될 수 있는 시간을 최소화 하기 위하여 유효기간이 짧게 설정되어있습니다.

#### 리프레시 토큰의 특징은 다음과 같습니다.

- 액세스 토큰이 발급될 때 함께 제공됩니다.
- 리프레시 토큰을 사용하여 액세스 토큰을 재발급 받을 수 있습니다.
- 리프레시 토큰의 유효기간은 14일입니다.
- 한 번 사용된 리프레시 토큰은 폐기됩니다.

![[Cafe24 Access Token.jpg]]


- 그림과 같이 액세스 토큰이 만료되더라도 함께 발급받았던 리프레시 토큰이 유효하다면, 인증 과정을 처음부터 반복하지 않아도 액세스 토큰을 재발급 받으실 수 있습니다.

**Caution!**

리프레시 토큰까지 전부 만료된 경우에는 인증 코드 요청 단계부터 다시 시작하셔야 합니다.

## 리프레시 토큰을 사용한 액세스 토큰 Request

- 리프레시 토큰을 사용하여 액세스 토큰을 재발급 합니다.

| Method | Resource URI                                       | Format                |
| ------ | -------------------------------------------------- | --------------------- |
| POST   | https://{mall_id}.cafe24api.com/api/v2/oauth/token | x-www-form-urlencoded |

### 리프레시 토큰을 사용한 액세스 토큰 Request 형식

```html
curl -X POST
'https://{mall_id}.cafe24api.com/api/v2/oauth/token'
 -H 'Authorization: Basic {base64_encode({client_id}:{client_Secret})}'
 -H 'Content-Type: application/x-www-form-urlencoded'
 -d 'grant_type=refresh_token'
 -d 'refresh_token={refresh_token}'
```

샘플

```
curl -X POST
'https://samplemall.cafe24api.com/api/v2/oauth/token'
 -H 'Authorization: Basic sampleRCTjdPVk5uQjNGMHM3UzFNRDpFaEZnM0xYak1KR21BZWV5MUliaXhI'
 -H 'Content-Type: application/x-www-form-urlencoded'
 -d 'grant_type=refresh_token'
 -d 'refresh_token=sample80BQWWCJEiwTHWCrU'
```

### 파라미터 정보

|파라미터|필수여부|설명|
|---|---|---|
|grant_type|필수|인증 형식입니다. "refresh_token"로 입력하세요.|
|refresh_token|필수|이전 액세스 토큰 발급 시 함께 제공받은 리프레시 토큰값을 입력하세요.|

## 액세스 토큰 Response

- Request에 성공하면 다음과 같은 응답을 확인하실 수 있습니다.

### 액세스 토큰 Response 형식

```json
HTTP/1.1 200 OK
{
    "access_token": "{access_token}",
    "expires_at": "{expiration_date_of_access_token}",
    "refresh_token": "{refresh_token}",
    "refresh_token_expires_at": "{expiration_date_of_refresh_token}",
    "client_id": "{client_id}",
    "mall_id": "{mall_id}",
    "user_id": "{user_id}",
    "scopes": [
        "{scopes_1}",
        "{scopes_2}"
    ],
    "issued_at": "{issued_date}"
}
```

**샘플 보기**열기

Caution!

Header의 {base64_encode({client_id}:{client_Secret})} 부분은 {클라이언트 아이디}:{클라이언트 시크릿}을 Base64 방식으로 인코딩하여 입력하세요.  
액세스 토큰을 요청할 때는 이와 같이 클라이언트 계정 정보를 함께 포함해야 하기 때문에, 리프레시 토큰만 탈취되는 경우에는 문제가 발생하지 않습니다.

### 파라미터 정보

|파라미터|설명|
|---|---|
|access_token|재발급 받은 액세스 토큰|
|expires_at|액세스 토큰 만료 일시/ 발급으로부터 2시간 후 만료됩니다.|
|refresh_token|액세스 토큰을 재발급 할 수 있는 토큰|
|refresh_token_expires_at|리프레시 토큰 만료 일시/ 발급으로부터 14일 후 만료됩니다.|
|client_id|클라이언트 아이디|
|mall_id|해당 쇼핑몰의 아이디|
|user_id|쇼핑몰 사용자(운영자) 아이디|
|scopes|사용 동의된 Scope 정보|
|issued_at|액세스 토큰 발급 일시|

**Caution!**

Date 관련 데이터는 ISO_8601 포맷(yyyy-mm-ddThh:mm:ss.sssZ)을 사용합니다.

## 에러 코드 안내

- Request에 실패하면 에러 코드를 반환합니다.

### 에러 코드 Response 형식

```json
HTTP/1.1 401 Unauthorized
{
    "error":"{error}",
    "error_description":"{error_description}"
}
```

**샘플 보기**열기

### 파라미터 정보

| 파라미터              | 설명                                               |
| ----------------- | ------------------------------------------------ |
| error             | OAuth 2.0 Framework Standard 5.2에 정의된 에러 코드값입니다. |
| error_description | 오류에 대한 자세한 설명입니다.                                |

### 에러 코드 안내

- 에러 코드 별 설명 및 그에 따른 대응 방법을 안내합니다.

| 에러 코드                  | HTTP Status                          | 설명                                         | 대응 방법                                                                                       |
| ---------------------- | ------------------------------------ | ------------------------------------------ | ------------------------------------------------------------------------------------------- |
| invalid_request        | 400                                  | refresh_token 값을 누락하여 요청한 경우               | 누락된 값을 확인하여 입력하세요.                                                                          |
| invalid_grant          | 400                                  | 잘못되거나 만료된 refresh_token을 요청한 경우            | 이전에 사용한 refresh_token 이거나 만료된 refresh_token 인지 확인하세요.  <br>인증 코드 요청을 참고하여 새로운 코드 발급을 진행하세요. |
| unsupported_grant_type | 400                                  | grant_type 값이 누락되거나 "refresh_token"가 아닐 경우 | 'grant_type=refresh_token' 로 되어있는지 확인하세요.                                                   |
| invalid_client         | 401                                  | client_id/ client_secret 값을 누락한 경우         | Authorization 값이 누락되었는지 확인하세요.                                                              |
| 401                    | 잘못된 client_id, client_secret로 요청한 경우 | 입력한 값이 개발자 어드민에 앱 기본정보와 동일한지 확인하세요.        |                                                                                             |


---

출처 - https://developers.cafe24.com/app/front/app/develop/oauth/retoken