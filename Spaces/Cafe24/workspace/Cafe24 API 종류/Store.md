
## Store API

![[cafe24 Store1.png]]


```
GET /api/v2/admin/store
```

상점(Store)은 쇼핑몰의 쇼핑몰명, 관리자 정보, 사업자 등록번호와 고객센터 전화번호 등 쇼핑몰의 기본적인 정보를 확인할 수 있는 기능입니다.


### Store properties 

| **Attribute**                        | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shop_no                              | 멀티쇼핑몰 번호<br><br>멀티쇼핑몰 구분을 위해 사용하는 멀티쇼핑몰 번호.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| shop_name                            | 쇼핑몰명<br><br>해당 상점의 쇼핑몰명([쇼핑몰 설정 > 기본 설정 > '쇼핑몰 정보 > 내 쇼핑몰 정보'])                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| mall_id                              | 상점 아이디<br><br>쇼핑몰 아이디. 대표운영자의 아이디이자, 쇼핑몰 기본 제공 도메인(mallid.cafe24.com)에 사용한다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| base_domain                          | 기본제공 도메인<br><br>쇼핑몰 생성시 자동으로 생성되는 기본제공 도메인 정보. 해당 도메인을 통해 쇼핑몰에 접근할 수 있다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| primary_domain                       | 대표도메인<br><br>쇼핑몰에 연결한 대표도메인. 대표도메인을 연결하였을 경우에만 노출된다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| company_registration_no              | 사업자등록번호<br><br>사업장이 위치한 국가에서 발급한 쇼핑몰의 사업자 등록 번호.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| company_name                         | 상호명<br><br>사업자 등록시 등록한 상호명 또는 법인명.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| president_name                       | 대표자명<br><br>사업자 등록시 등록한 대표자명.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| company_condition                    | 업태<br><br>사업자 등록시 등록한 업태.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| company_line                         | 종목<br><br>사업자 등록시 등록한 종목.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| country                              | 사업장 국가<br><br>사업장이 있는 국가명.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| country_code                         | 국가코드                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| zipcode                              | 우편번호<br><br>사업장의 우편번호                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| address1                             | 기본 주소<br><br>사업장 주소(시/군/도)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| address2                             | 상세 주소<br><br>사업장 주소(상세 주소)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| phone                                | 전화번호                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| fax                                  | 팩스번호                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| email                                | 이메일<br><br>운영자가 자동메일을 수신할 경우 수신할 메일 주소                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| notification_only_email              | 발신전용 이메일<br><br>고객과 운영자에게 자동메일 발송시 보내는 사람 메일주소                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| mall_url                             | 쇼핑몰 주소                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| mail_order_sales_registration        | 통신 판매업 신고<br><br>통신판매업 신고가 되었는지 신고 여부<br><br>T : 신고함  <br>F : 신고안함                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| mail_order_sales_registration_number | 통신판매신고 번호                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| missing_report_reason_type           | 통신판매업 미신고 사유<br><br>통신판매업 신고를 하지 않았을 경우 해당 사유.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| missing_report_reason                | 통신판매업 미신고 사유 상세 내용<br><br>통신판매업 미신고 사유가 "기타"일 경우 상세 사유.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| about_us_contents                    | 회사소개<br><br>쇼핑몰에 대한 간략한 소개 표시. 쇼핑몰의 회사 소개 화면에 표시된다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| company_map_url                      | 회사약도<br><br>쇼핑몰에 대한 간략한 약도 표시. 쇼핑몰의 회사 소개 화면에 표시된다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| customer_service_phone               | 고객센터 상담/주문 전화<br><br>쇼핑몰 화면에 표시되는 고객센터 상담 전화                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| customer_service_email               | 고객센터 상담/주문 이메일<br><br>쇼핑몰 화면에 표시되는 고객센터 상담 이메일 주소.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| customer_service_fax                 | 고객센터 팩스 번호<br><br>쇼핑몰 화면에 표시되는 고객센터 팩스 번호.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| customer_service_sms                 | 고객센터 SMS 수신번호<br><br>쇼핑몰 화면에 표시되는 고객센터 SMS 수신 번호.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| customer_service_hours               | 고객센터 운영시간<br><br>쇼핑몰 화면에 표시되는 고객센터 운영시간.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| privacy_officer_name                 | 개인정보보호 책임자명<br><br>쇼핑몰 화면에 표시되는 개인정보보호 책임자 이름.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| privacy_officer_position             | 개인정보보호 책임자 지위                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| privacy_officer_department           | 개인정보보호 책임자 부서                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| privacy_officer_phone                | 개인정보보호 책임자 연락처<br><br>쇼핑몰 화면에 표시되는 개인정보보호 책임자의 전화번호.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| privacy_officer_email                | 개인정보보호 책임자 이메일<br><br>쇼핑몰 화면에 표시되는 개인정보보호 책임자의 이메일 주소.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| contact_us_mobile                    | 서비스 문의안내 모바일 표시여부<br><br>서비스 문의 안내를 모바일에 노출시킬 것인지 여부.<br><br>T : 표시함  <br>F : 표시안함                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| contact_us_contents                  | 서비스 문의안내 내용<br><br>상품상세 페이지에 노출시키는 서비스 문의 안내 내용.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| sales_product_categories             | 판매 상품 카테고리<br><br>회원가입 및 쇼핑몰 생성 직후 입력하는 판매 상품 카테고리의 정보를 조회할 수 있습니다.  <br>  <br>(2023년 4월 이후 가입한 몰에 한하여 조회할 수 있습니다.)<br><br>Undecided : 아직 결정하지 못했어요.  <br>Apparel : 패션의류  <br>FashionAccessories : 패션잡화  <br>LuxuryGoods : 수입명품  <br>BrandApparel : 브랜드의류  <br>BrandAccessories : 브랜드잡화  <br>Food_Beverage : 식품  <br>Lifestyle_HealthCare : 생활/건강  <br>Furniture_HomeDecor : 가구/인테리어  <br>Beauty_PersonalCare : 화장품/미용  <br>Maternity_BabyProducts : 출산/육아  <br>Digital_HomeAppliances : 디지털/가전  <br>CarAccessories : 자동차  <br>Rentals : 렌탈 서비스  <br>Sports_Leisure : 스포츠/레저  <br>CD_DVD : 음반/DVD  <br>Books : 도서  <br>Travels_Services : 여가/생활편의  <br>Used_Refurbished_Exhibition : 중고/리퍼/ |


## Retrieve store details

쇼핑몰의 정보를 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

| **Parameter** | **Description**                                              |
| ------------- | ------------------------------------------------------------ |
| shop_no       | 멀티쇼핑몰 번호<br><br>멀티쇼핑몰 구분을 위해 사용하는 멀티쇼핑몰 번호.<br><br>DEFAULT 1 |

request
```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://{mallid}.cafe24api.com/api/v2/admin/store?shop_no=1',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: Bearer {access_token}',
    'Content-Type: application/json',
    'X-Cafe24-Api-Version: {version}'
  ),
));
$response = curl_exec($curl);
$err = curl_error($curl);
if ($err) {
  echo 'cURL Error #:' . $err;
} else {
  echo $response;
}
```


response
```json
{
    "store": {
        "shop_no": 1,
        "shop_name": "My Shopping Mall",
        "mall_id": "myshop",
        "base_domain": "sample.cafe24.com",
        "primary_domain": "sample.com",
        "company_registration_no": "118-81-20586",
        "company_name": "My Shopping Mall",
        "president_name": "John Doe",
        "company_condition": "Retail",
        "company_line": "E-Commerce Product",
        "country": "Korea",
        "country_code": "KOR",
        "zipcode": "07071",
        "address1": "Sindaebang dong Dongjak-gu, Seoul, Republic of Korea",
        "address2": "Professional Construction Hall",
        "phone": "02-0000-0000",
        "fax": "02-0000-0000",
        "email": "sample@sample.com",
        "notification_only_email": "sample@sample.com",
        "mall_url": "http://sample.com",
        "mail_order_sales_registration": "T",
        "mail_order_sales_registration_number": "강남 제 02-680-014호",
        "missing_report_reason_type": "Preparing for Register",
        "missing_report_reason": "Preparing to report ecommerce business",
        "about_us_contents": "<b>My Shopping Mall Information</b>",
        "company_map_url": "https://myshop.cafe24.com/web/upload/map.jpg",
        "customer_service_phone": "02-0000-0000",
        "customer_service_email": "sample@sample.com",
        "customer_service_fax": "02-0000-0000",
        "customer_service_sms": "02-0000-0000",
        "customer_service_hours": "9:00 AM ~ 5:00 PM",
        "privacy_officer_name": "Hong Gildong",
        "privacy_officer_position": "Manager",
        "privacy_officer_department": "Information Security Team",
        "privacy_officer_phone": "02-0000-0000",
        "privacy_officer_email": "sample@sample.com",
        "contact_us_mobile": "T",
        "contact_us_contents": "Service Information",
        "sales_product_categories": [
            "Apparel",
            "FashionAccessories"
        ]
    }
}
```



## Store accounts


![[cafe24 Store2.png]]


상점 계좌(Store accounts)는 쇼핑몰의 무통장입금 정보에 대한 기능입니다.


```
GET /api/v2/admin/store/accounts
```


### Store accounts properties [](https://developers.cafe24.com/docs/ko/api/admin/#store-accounts-properties)

| **Attribute**                      | **Description**                                                                                                                                                 |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shop_no                            | 멀티쇼핑몰 번호                                                                                                                                                        |
| bank_account_id                    | 무통장 입금 은행 ID                                                                                                                                                    |
| bank_name                          | 은행명                                                                                                                                                             |
| bank_code  <br><br>_최대글자수 : [50자]_ | 은행코드<br><br>[bank_code](https://d2wxkjpieznxai.cloudfront.net/resource/ko/bank_code.xlsx) ![](https://d2wxkjpieznxai.cloudfront.net/resource/ko/excel_icon.png) |
| bank_account_no                    | 계좌번호                                                                                                                                                            |
| bank_account_holder                | 예금주                                                                                                                                                             |
| use_account                        | 사용여부<br><br>T : 사용함  <br>F : 사용안함                                                                                                                               |
|                                    |                                                                                                                                                                 |
|                                    |                                                                                                                                                                 |
|                                    |                                                                                                                                                                 |


### Retrieve a list of store bank accounts 

GET/api/v2/admin/store/accounts

상점의 무통장입금 계좌정보를 목록으로 조회할 수 있습니다.  
은행명, 은행코드, 계좌번호 등을 조회할 수 있습니다.



#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

| **Parameter** | **Description**           |
| ------------- | ------------------------- |
| shop_no       | 멀티쇼핑몰 번호<br><br>DEFAULT 1 |

```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/store/accounts")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```


```json
{
    "accounts": [
        {
            "shop_no": 1,
            "bank_account_id": 1,
            "bank_name": "Hana Bank",
            "bank_code": "bank_81",
            "bank_account_no": "123123123",
            "bank_account_holder": "Depositor Name",
            "use_account": "T"
        },
        {
            "shop_no": 1,
            "bank_account_id": 2,
            "bank_name": "KB Bank",
            "bank_code": "bank_04",
            "bank_account_no": "123456789",
            "bank_account_holder": "Depositor Name",
            "use_account": "T"
        }
    ]
}
```



## Activitylogs

활동로그(Activitylog)는 쇼핑몰 관리자가 쇼핑몰 어드민에서 진행한 운영 활동을 기록한 내역입니다.  
활동로그 리소스를 사용하면 쇼핑몰의 활동로그를 생성하거나 조회할 수 있습니다.

```
GET /api/v2/admin/activitylogs
GET /api/v2/admin/activitylogs/{process_no}
```



### Activitylogs properties 

|**Attribute**|**Description**|
|---|---|
|process_no|업무처리 넘버|
|mode|모드<br><br>P : PC 어드민  <br>M : 모바일 어드민  <br>S : (구)스마트모드|
|type|구분|
|content|업무내용|
|process_date|처리일시|
|manager_id  <br><br>_형식 : [a-z0-9]_  <br>_글자수 최소: [4자]~최대: [16자]_|처리자|
|manager_type|처리자 타입|



### Retrieve a list of action logs 

GET/api/v2/admin/activitylogs

활동로그를 목록으로 조회할 수 있습니다.  
운영 활동을 한 사람이 누구인지, 어떤 메뉴에서 언제 진행했는지 확인할 수 있습니다.

특정 클라이언트만 사용 가능

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://{mallid}.cafe24api.com/api/v2/admin/activitylogs?start_date=2020-01-01T00:00:00+09:00&end_date=2020-03-01T23:59:59+09:00',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: Bearer {access_token}',
    'Content-Type: application/json',
    'X-Cafe24-Api-Version: {version}'
  ),
));
$response = curl_exec($curl);
$err = curl_error($curl);
if ($err) {
  echo 'cURL Error #:' . $err;
} else {
  echo $response;
}
```

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|manager_type|처리자 타입<br><br>P : 대표운영자  <br>A : 부운영자  <br>S : 공급사|
|manager_id  <br><br>_형식 : [a-z0-9]_  <br>_글자수 최소: [4자]~최대: [16자]_|처리자|
|mode|모드<br><br>P : PC 어드민  <br>M : 모바일 어드민  <br>S : (구)스마트모드|
|type|구분|
|content  <br><br>_최대글자수 : [500자]_|업무내용|
|**start_date**  <br>**Required**  <br><br>_날짜_|검색 시작일|
|**end_date**  <br>**Required**  <br><br>_날짜_|검색 종료일|
|offset  <br><br>_최대값: [8000]_|조회결과 시작위치<br><br>DEFAULT 0|
|limit  <br><br>_최소: [1]~최대: [100]_|조회결과 최대건수<br><br>DEFAULT 10|


```json
{
    "activitylogs": [
        {
            "process_no": 130,
            "mode": "P",
            "type": "product management > product management > product list",
            "content": "Edit product name",
            "process_date": "2020-02-01T00:00:00+09:00",
            "manager_id": "sampleid",
            "manager_type": "representative operator"
        },
        {
            "process_no": 131,
            "mode": "P",
            "type": "product management > product management > product list",
            "content": "Edit product name",
            "process_date": "2020-02-02T00:00:00+09:00",
            "manager_id": "sampleid",
            "manager_type": "representative operator"
        }
    ],
    "links": [
        {
            "rel": "next",
            "href": "https://{mallid}.cafe24api.com/api/v2/admin/activitylogs?limit=10&offset=10"
        }
    ]
}
```



#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|**process_no**  <br>**Required**|업무처리 넘버|
```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://{mallid}.cafe24api.com/api/v2/admin/activitylogs/130',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: Bearer {access_token}',
    'Content-Type: application/json',
    'X-Cafe24-Api-Version: {version}'
  ),
));
$response = curl_exec($curl);
$err = curl_error($curl);
if ($err) {
  echo 'cURL Error #:' . $err;
} else {
  echo $response;
}
```


```json
{
    "activitylog": {
        "process_no": 130,
        "type": "product management > product management > product list",
        "manager_id": "sampleid",
        "manager_type": "representative operator",
        "process_date": "2020-02-01T00:00:00+09:00",
        "content": "Edit product name"
    }
}
```

## Automessages arguments

자동메시지 변수(Automessages arguments)는 자동메시지 발신 시 사용할 수 있는 변수를 확인하는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/automessages/arguments
```

### Automessages arguments properties 

|**Attribute**|**Description**|
|---|---|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|name|변수명|
|description|변수 설명|
|sample|변수 예제|
|string_length|메시지 표시 최대 글자수<br><br>글자수 : 설정된 글자수 만큼 표시  <br>가변 : 글자수 제한 없이 모두 표시|
|send_case|사용 가능 발송 상황|
### Retrieve the list of available variables for automated messages [](https://developers.cafe24.com/docs/ko/api/admin/#retrieve-the-list-of-available-variables-for-automated-messages)

GET/api/v2/admin/automessages/arguments

자동메시지 발송 설정내역을 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호<br><br>DEFAULT 1|

Retrieve the list of available variables for automated messages

> RequestcURL

```php
<?php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => 'https://{mallid}.cafe24api.com/api/v2/admin/automessages/arguments',
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_CUSTOMREQUEST => 'GET',
  CURLOPT_HTTPHEADER => array(
    'Authorization: Bearer {access_token}',
    'Content-Type: application/json',
    'X-Cafe24-Api-Version: {version}'
  ),
));
$response = curl_exec($curl);
$err = curl_error($curl);
if ($err) {
  echo 'cURL Error #:' . $err;
} else {
  echo $response;
}
```

> Response

```json
{
    "arguments": [
        {
            "shop_no": 1,
            "name": "[NAME]",
            "description": "Customer name",
            "sample": "John Doe",
            "string_length": "3",
            "send_case": "All occasions"
        },
        {
            "shop_no": 1,
            "name": "[PRODUCT]",
            "description": "Product name",
            "sample": "iPhone X",
            "string_length": "8",
            "send_case": "Back-in-stock notification (Manual), Notification on shipment, Successful delivery, or Refund"
        }
    ]
}
```

## Automessages setting

자동메시지 설정(Automessages setting)은 메시지 자동 발송 시 사용 중인 발송 수단을 확인 및 어떤 발송 수단으로 우선발송할 지 조회, 변경하는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/automessages/setting
PUT /api/v2/admin/automessages/setting
```

더보기

## Coupons setting

쿠폰 설정(Coupons setting)은 쇼핑몰에서 사용할 쿠폰의 기본적인 설정을 입력할 수 있습니다.  
쿠폰의 할인, 적립 기능의 사용여부와 제한조건, 진열 등 다양한 측면의 설정이 가능합니다.

> Endpoints

```
GET /api/v2/admin/coupons/setting
PUT /api/v2/admin/coupons/setting
```

더보기

## Currency

환율 정보(Currency)는 쇼핑몰의 화폐 정보, 환율 정보 등을 확인할 수 있는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/currency
```

더보기

## Dashboard

대시보드(Dashboard)는 쇼핑몰의 주문 현황과 매출 현황 등 쇼핑몰 운영에 필요한 정보를 간략하게 요약해놓은 정보입니다.

> Endpoints

```
GET /api/v2/admin/dashboard
```

더보기

## Financials paymentgateway

Financials paymentgateway(PG 정보)는 PG사별 계약정보를 제공합니다.

> Endpoints

```
GET /api/v2/admin/financials/paymentgateway
```

더보기

## Financials store

Financials store(상점의 거래정보)는 상점의 PG사별 거래정보를 제공합니다.

> Endpoints

```
GET /api/v2/admin/financials/store
```

더보기

## Images setting

상품 이미지 사이즈 설정 값을 조회하거나 수정할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/images/setting
PUT /api/v2/admin/images/setting
```

더보기

## Kakaoalimtalk profile

상점의 카카오채널 프로필키 등록여부를 확인할 수 있는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/kakaoalimtalk/profile
```

더보기

## Kakaoalimtalk setting

카카오알림톡 서비스(Kakaoalimtalk setting) 사용 여부를 조회하고 설정을 변경하는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/kakaoalimtalk/setting
PUT /api/v2/admin/kakaoalimtalk/setting
```

더보기

## Kakaopay setting

쇼핑몰의 카카오페이 설정을 조회하거나 수정할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/kakaopay/setting
PUT /api/v2/admin/kakaopay/setting
```

더보기

## Menus

메뉴(Menus)는 쇼핑몰의 메뉴 모드에 관한 기능입니다.  
쇼핑몰의 메뉴 모드와 경로 등을 조회할 수 있습니다.  
쇼핑몰의 메뉴 모드로는 프로모드, 스마트모드, 모바일 어드민이 있습니다.

> Endpoints

```
GET /api/v2/admin/menus
```

더보기

## Mobile setting

모바일 설정(Mobile setting)은 쇼핑몰의 모바일 쇼핑몰 설정에 관한 리소스입니다.  
모바일 쇼핑몰 사용 여부와 접속 주소 자동연결의 사용/사용안함 여부를 조회할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/mobile/setting
```

더보기

## Naverpay setting

네이버페이 설정(Naverpay Setting)은 쇼핑몰의 네이버페이 공통인증키를 조회하거나 수정할 수 있는 기능입니다.

> Endpoints

```
GET /api/v2/admin/naverpay/setting
POST /api/v2/admin/naverpay/setting
PUT /api/v2/admin/naverpay/setting
```

더보기

## Orders setting

취소/반품시 자동 수량 복구 및 할인/적립 금액 등 주문 설정에 대해 조회, 수정 할 수 있는 기능입니다.

> Endpoints

```
GET /api/v2/admin/orders/setting
PUT /api/v2/admin/orders/setting
```

더보기

## Orders status

> Endpoints

```
GET /api/v2/admin/orders/status
PUT /api/v2/admin/orders/status
```

더보기

## Payment setting

> Endpoints

```
GET /api/v2/admin/payment/setting
PUT /api/v2/admin/payment/setting
```

더보기

## Paymentgateway

  ![[cafe24 Store3.png]]
  
PG(Paymentgateway)를 통해 PG앱의 조회, 등록, 수정, 삭제가 가능합니다.

> Endpoints

```
POST /api/v2/admin/paymentgateway
PUT /api/v2/admin/paymentgateway/{client_id}
DELETE /api/v2/admin/paymentgateway/{client_id}
```

더보기

## Paymentgateway paymentmethods

> Endpoints

```
GET /api/v2/admin/paymentgateway/{client_id}/paymentmethods
POST /api/v2/admin/paymentgateway/{client_id}/paymentmethods
PUT /api/v2/admin/paymentgateway/{client_id}/paymentmethods/{payment_method_code}
DELETE /api/v2/admin/paymentgateway/{client_id}/paymentmethods/{payment_method_code}
```

더보기

## Paymentmethods

  ![[cafe24 Store4.png]]
  
쇼핑몰에 설정된 결제수단을 조회할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/paymentmethods
```

더보기

## Paymentmethods paymentproviders

> Endpoints

```
GET /api/v2/admin/paymentmethods/{code}/paymentproviders
PUT /api/v2/admin/paymentmethods/{code}/paymentproviders/{name}
```

더보기

## Points setting

적립금 설정(Points setting)은 적립금 사용에 필요한 설정값을 관리하기 위한 기능입니다.

> Endpoints

```
GET /api/v2/admin/points/setting
PUT /api/v2/admin/points/setting
```

더보기

## Products setting

상품의 설정(Products setting)은 상품의 판매가 등의 설정값에 대한 기능입니다.

> Endpoints

```
GET /api/v2/admin/products/setting
```

더보기

## Redirects

특정 URL로 접속 했을때, 설정한 URL로 리다이렉트할 수 있는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/redirects
POST /api/v2/admin/redirects
PUT /api/v2/admin/redirects/{id}
DELETE /api/v2/admin/redirects/{id}
```

더보기

## Seo setting

SEO 설정(Seo setting)은 검색결과 상위에 쇼핑몰이 노출되고 방문자가 증가하도록 하는 검색엔진 최적화(SEO) 작업입니다.

> Endpoints

```
GET /api/v2/admin/seo/setting
PUT /api/v2/admin/seo/setting
```

더보기

## Shippingmanager

배송 관리자(Shippingmanager)는 배송 관리자 활성화 정보 관련 기능입니다.  
배송 관리자의 사용 정보를 조회할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/shippingmanager
```

더보기

## Shops

멀티쇼핑몰(Shops)은 한개의 몰아이디에서 두개 이상의 쇼핑몰을 운영하고 있는 경우 생성한 멀티 쇼핑몰의 정보입니다.  
멀티쇼핑몰은 최대 15개까지 생성이 가능하며, 각각 다른 언어와 화폐로 생성할 수 있어 다국어 쇼핑몰을 운영하기 용이합니다.

> Endpoints

```
GET /api/v2/admin/shops
GET /api/v2/admin/shops/{shop_no}
```

더보기

## Sms setting

SMS 설정(Sms setting)은 쇼핑몰의 SMS 설정에 관한 기능입니다.  
SMS API를 사용하기 위해서는 먼저 쇼핑몰에서 SMS 발송 서비스를 사용하고 있는지 확인이 필요합니다.

> Endpoints

```
GET /api/v2/admin/sms/setting
```

더보기

## Socials apple

애플아이디 로그인(Socials apple)은 쇼핑몰 이용 고객의 애플아이디 로그인에 관한 기능입니다.  
애플아이디 로그인 설정을 사용하기 위해서는 먼저 애플의 개발자 계정에서 Sign in with Apple 앱 설정을 완료하여야 합니다.

> Endpoints

```
GET /api/v2/admin/socials/apple
PUT /api/v2/admin/socials/apple
```

더보기

## Socials kakaosync

카카오싱크 SNS(Socials kakaosync)는 쇼핑몰의 카카오싱크에 대한 설정을 조회하거나 설정할 수 있는 기능입니다.

> Endpoints

```
GET /api/v2/admin/socials/kakaosync
PUT /api/v2/admin/socials/kakaosync
```

더보기

## Socials naverlogin

네이버 로그인 설정정보를 조회하고 설정정보를 변경하는 리소스입니다.

> Endpoints

```
GET /api/v2/admin/socials/naverlogin
PUT /api/v2/admin/socials/naverlogin
```



### Socials naverlogin properties 

|**Attribute**|**Description**|
|---|---|
|shop_no|멀티쇼핑몰 번호|
|use_naverlogin|네이버 로그인 사용여부|
|client_id|클라이언트 아이디|
|client_secret|클라이언트 시크릿 키|

### Naver login details 

GET/api/v2/admin/socials/naverlogin

쇼핑몰에 네이버 로그인 설정여부를 조회할 수 있습니다.

`해당 API는 특정 클라이언트만 사용할 수 있는 API입니다. 사용하시려면 카페24 개발자센터로 문의해주세요.`

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호<br><br>DEFAULT 1|

Naver login details


```
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/socials/naverlogin")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```


```
{
    "naverlogin": {
        "shop_no": 1,
        "use_naverlogin": "T",
        "client_id": "d3t09cT11SNX22U5swHK",
        "client_secret": "XxT3QPuMkU"
    }
}
```

### Update Naver login settings 

PUT/api/v2/admin/socials/naverlogin

쇼핑몰에 설정된 네이버 로그인 정보를 수정할 수 있습니다.

`해당 API는 특정 클라이언트만 사용할 수 있는 API입니다. 사용하시려면 카페24 개발자센터로 문의해주세요.`

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 쓰기권한 (mall.write_store)**|
|호출건수 제한|**40**|
|1회당 요청건수 제한|**1**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|**use_naverlogin**  <br>**Required**|네이버 로그인 사용여부<br><br>T:사용함  <br>F:사용안함|
|client_id  <br><br>_형식 : [a-zA-Z0-9_-]_  <br>_최대글자수 : [255자]_|클라이언트 아이디|
|client_secret  <br><br>_형식 : [a-zA-Z0-9_-]_  <br>_최대글자수 : [255자]_|클라이언트 시크릿 키|

Update Naver login settings

```
MediaType mediaType = MediaType.parse("");
RequestBody body = RequestBody.create(mediaType, "{\n" +
"    \"shop_no\": 1,\n" +
"    \"request\": {\n" +
"        \"use_naverlogin\": \"T\",\n" +
"        \"client_id\": \"d3t09cT11SNX22U5swHK\",\n" +
"        \"client_secret\": \"XxT3QPuMkU\"\n" +
"    }\n" +
"}");
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/socials/naverlogin")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .put(body)
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```
{
    "naverlogin": {
        "shop_no": 1,
        "use_naverlogin": "T",
        "client_id": "d3t09cT11SNX22U5swHK",
        "client_secret": "XxT3QPuMkU"
    }
}
```



## Subscription shipments setting

정기배송 설정(Subscription shipments setting)은 정기결제를 통해 이루어지는 정기배송에 대한 기능입니다.  
정기배송 설정을 통해 쇼핑몰의 정기배송 상품을 설정하거나 정기배송 상품을 조회할 수 있습니다.  
정기배송 기능을 사용하기 위해서는 먼저 정기배송 서비스가 신청되어 있어야 합니다.  
정기배송 서비스의 신청은 어드민에서 가능합니다.

> Endpoints

```
GET /api/v2/admin/subscription/shipments/setting
POST /api/v2/admin/subscription/shipments/setting
PUT /api/v2/admin/subscription/shipments/setting/{subscription_no}
DELETE /api/v2/admin/subscription/shipments/setting/{subscription_no}
```

### Subscription shipments setting properties 

| **Attribute**                                             | **Description**                                                                                                                                             |
| --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| shop_no                                                   | 멀티쇼핑몰 번호                                                                                                                                                    |
| subscription_no                                           | 정기배송 상품설정 번호                                                                                                                                                |
| subscription_shipments_name                               | 정기배송 상품설정 명                                                                                                                                                 |
| product_binding_type                                      | 정기배송 상품 설정<br><br>A : 전체상품  <br>P : 개별상품  <br>C : 상품분류                                                                                                      |
| one_time_purchase                                         | 1회구매 제공여부<br><br>T : 제공함  <br>F : 제공안함                                                                                                                      |
| product_list                                              | 적용 상품                                                                                                                                                       |
| category_list                                             | 적용 분류                                                                                                                                                       |
| use_discount                                              | 정기배송 할인 사용여부<br><br>T : 사용함  <br>F : 사용안함                                                                                                                   |
| discount_value_unit                                       | 할인 기준<br><br>P : 할인율  <br>W : 할인 금액                                                                                                                         |
| discount_values                                           | 할인 값                                                                                                                                                        |
| related_purchase_quantity                                 | 구매수량 관계 여부<br><br>T : 구매수량에 따라  <br>F : 구매수량에 관계없이                                                                                                          |
| subscription_shipments_cycle_type                         | 배송주기 제공여부<br><br>T : 사용함  <br>F : 사용안함                                                                                                                      |
| subscription_shipments_cycle                              | 배송주기<br><br>1W : 1주  <br>2W : 2주  <br>3W : 3주  <br>4W : 4주  <br>1M : 1개월  <br>2M : 2개월  <br>3M : 3개월  <br>4M : 4개월  <br>5M : 5개월  <br>6M : 6개월  <br>1Y : 1년 |
| use_order_price_condition                                 | 혜택제공금액기준 사용여부<br><br>T : 사용함  <br>F : 사용안함                                                                                                                  |
| order_price_greater_than                                  | 혜택제공금액기준 제공 기준금액                                                                                                                                            |
| include_regional_shipping_rate                            | 지역별배송비 포함여부<br><br>T : 포함  <br>F : 미포함                                                                                                                      |
| shipments_start_date  <br><br>_최소값: [1]_  <br>_최대값: [30]_ | 배송시작일 설정                                                                                                                                                    |
| change_option                                             | 옵션 변경 가능 여부<br><br>T : 사용함  <br>F : 사용안함                                                                                                                    |

> Endpoints

```
GET /api/v2/admin/subscription/shipments/setting
POST /api/v2/admin/subscription/shipments/setting
PUT /api/v2/admin/subscription/shipments/setting/{subscription_no}
DELETE /api/v2/admin/subscription/shipments/setting/{subscription_no}
```


### Retrieve a list of subscription products 

GET/api/v2/admin/subscription/shipments/setting

설정된 정기배송 상품에 대한 정보를 목록으로 조회할 수 있습니다.  
정기배송 상품설정 번호, 설정 명, 설정값 등을 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|subscription_no|정기배송 상품설정 번호|


```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/subscription/shipments/setting")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```


```json
{
    "shipments": [
        {
            "shop_no": 1,
            "subscription_no": 70,
            "subscription_shipments_name": "SHIRTS SUBSCRIPTION SHIPMENTS",
            "product_binding_type": "P",
            "one_time_purchase": "T",
            "product_list": [
                11,
                13
            ],
            "category_list": null,
            "use_discount": "T",
            "discount_value_unit": "P",
            "discount_values": [
                "20.00",
                "11.00"
            ],
            "subscription_shipments_cycle_type": "T",
            "subscription_shipments_cycle": [
                "1M",
                "2M"
            ],
            "use_order_price_condition": "T",
            "order_price_greater_than": "25000.00",
            "include_regional_shipping_rate": "F",
            "shipments_start_date": 3,
            "change_option": "F"
        },
        {
            "shop_no": 1,
            "subscription_no": 71,
            "subscription_shipments_name": "SHIRTS SUBSCRIPTION SHIPMENTS",
            "product_binding_type": "P",
            "one_time_purchase": "T",
            "product_list": [
                11,
                13
            ],
            "category_list": null,
            "use_discount": "T",
            "discount_value_unit": "P",
            "discount_values": [
                "20.00",
                "11.00"
            ],
            "subscription_shipments_cycle_type": "T",
            "subscription_shipments_cycle": [
                "1M",
                "2M"
            ],
            "use_order_price_condition": "T",
            "order_price_greater_than": "25000.00",
            "include_regional_shipping_rate": "F",
            "shipments_start_date": 3,
            "change_option": "T"
        }
    ]
}
```


### Create a subscription payment rule 

POST/api/v2/admin/subscription/shipments/setting

정기배송 상품을 설정할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 쓰기권한 (mall.write_store)**|
|호출건수 제한|**40**|
|1회당 요청건수 제한|**1**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|**subscription_shipments_name**  <br>**Required**  <br><br>_최대글자수 : [255자]_|정기배송 상품설정 명|
|**product_binding_type**  <br>**Required**|정기배송 상품 설정<br><br>A : 전체상품  <br>P : 개별상품  <br>C : 상품분류|
|one_time_purchase|1회구매 제공여부<br><br>T : 제공함  <br>F : 제공안함<br><br>DEFAULT T|
|product_list  <br><br>_배열 최대사이즈: [10000]_|적용 상품|
|category_list  <br><br>_배열 최대사이즈: [1000]_|적용 분류|
|**use_discount**  <br>**Required**|정기배송 할인 사용여부<br><br>T : 사용함  <br>F : 사용안함|
|discount_value_unit|할인 기준<br><br>P : 할인율  <br>W : 할인 금액|
|discount_values  <br><br>_배열 최대사이즈: [40]_|할인 값<br><br>discount_value_unit이 P일 경우 최대값 : 100  <br>discount_value_unit이 W일 경우 최대값 : 99999999999999|
|related_purchase_quantity|구매수량 관계 여부<br><br>T : 구매수량에 따라  <br>F : 구매수량에 관계없이|
|**subscription_shipments_cycle_type**  <br>**Required**|배송주기 제공여부<br><br>T : 사용함  <br>F : 사용안함|
|**subscription_shipments_cycle**  <br>**Required**|배송주기<br><br>1W : 1주  <br>2W : 2주  <br>3W : 3주  <br>4W : 4주  <br>1M : 1개월  <br>2M : 2개월  <br>3M : 3개월  <br>4M : 4개월  <br>5M : 5개월  <br>6M : 6개월  <br>1Y : 1년|
|**use_order_price_condition**  <br>**Required**|혜택제공금액기준 사용여부<br><br>T : 사용함  <br>F : 사용안함|
|order_price_greater_than  <br><br>_최대값: [99999999999999]_|혜택제공금액기준 제공 기준금액|
|include_regional_shipping_rate|지역별배송비 포함여부<br><br>T : 포함  <br>F : 미포함|
|shipments_start_date  <br><br>_최소값: [1]_  <br>_최대값: [30]_|배송시작일 설정<br><br>DEFAULT 3|
|change_option|옵션 변경 가능 여부<br><br>T : 사용함  <br>F : 사용안함<br><br>DEFAULT F|

Create a subscription payment rule

> RequestcURL 

```java
MediaType mediaType = MediaType.parse("");
RequestBody body = RequestBody.create(mediaType, "{\n" +
"    \"shop_no\": 1,\n" +
"    \"request\": {\n" +
"        \"subscription_shipments_name\": \"SHIRTS SUBSCRIPTION SHIPMENTS\",\n" +
"        \"product_binding_type\": \"P\",\n" +
"        \"one_time_purchase\": \"T\",\n" +
"        \"product_list\": [\n" +
"            11,\n" +
"            13\n" +
"        ],\n" +
"        \"category_list\": null,\n" +
"        \"use_discount\": \"T\",\n" +
"        \"discount_value_unit\": \"P\",\n" +
"        \"discount_values\": [\n" +
"            \"20.00\",\n" +
"            \"11.00\"\n" +
"        ],\n" +
"        \"subscription_shipments_cycle_type\": \"T\",\n" +
"        \"subscription_shipments_cycle\": [\n" +
"            \"1M\",\n" +
"            \"2M\"\n" +
"        ],\n" +
"        \"use_order_price_condition\": \"T\",\n" +
"        \"order_price_greater_than\": \"25000.00\",\n" +
"        \"include_regional_shipping_rate\": \"F\",\n" +
"        \"shipments_start_date\": 3,\n" +
"        \"change_option\": \"F\"\n" +
"    }\n" +
"}");
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/subscription/shipments/setting")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .post(body)
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "shipment": {
        "shop_no": 1,
        "subscription_no": 70,
        "subscription_shipments_name": "SHIRTS SUBSCRIPTION SHIPMENTS",
        "product_binding_type": "P",
        "one_time_purchase": "T",
        "product_list": [
            11,
            13
        ],
        "category_list": null,
        "use_discount": "T",
        "discount_value_unit": "P",
        "discount_values": [
            "20.00",
            "11.00"
        ],
        "subscription_shipments_cycle_type": "T",
        "subscription_shipments_cycle": [
            "1M",
            "2M"
        ],
        "use_order_price_condition": "T",
        "order_price_greater_than": "25000.00",
        "include_regional_shipping_rate": "F",
        "shipments_start_date": 3,
        "change_option": "F"
    }
}
```

### Update subscription products 

PUT/api/v2/admin/subscription/shipments/setting/{subscription_no}

설정된 정기배송 상품의 정기배송 설정을 수정할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 쓰기권한 (mall.write_store)**|
|호출건수 제한|**40**|
|1회당 요청건수 제한|**1**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|**subscription_no**  <br>**Required**|정기배송 상품설정 번호|
|subscription_shipments_name  <br><br>_최대글자수 : [255자]_|정기배송 상품설정 명|
|product_binding_type|정기배송 상품 설정<br><br>A : 전체상품  <br>P : 개별상품  <br>C : 상품분류|
|one_time_purchase|1회구매 제공여부<br><br>T : 제공함  <br>F : 제공안함|
|product_list  <br><br>_배열 최대사이즈: [10000]_|적용 상품|
|category_list  <br><br>_배열 최대사이즈: [1000]_|적용 분류|
|use_discount|정기배송 할인 사용여부<br><br>T : 사용함  <br>F : 사용안함|
|discount_value_unit|할인 기준<br><br>P : 할인율  <br>W : 할인 금액|
|discount_values  <br><br>_배열 최대사이즈: [40]_|할인 값|
|related_purchase_quantity|구매수량 관계 여부<br><br>T : 구매수량에 따라  <br>F : 구매수량에 관계없이|
|subscription_shipments_cycle_type|배송주기 제공여부<br><br>T : 사용함  <br>F : 사용안함|
|subscription_shipments_cycle|배송주기<br><br>1W : 1주  <br>2W : 2주  <br>3W : 3주  <br>4W : 4주  <br>1M : 1개월  <br>2M : 2개월  <br>3M : 3개월  <br>4M : 4개월  <br>5M : 5개월  <br>6M : 6개월  <br>1Y : 1년|
|use_order_price_condition|혜택제공금액기준 사용여부<br><br>T : 사용함  <br>F : 사용안함|
|order_price_greater_than  <br><br>_최대값: [99999999999999]_|혜택제공금액기준 제공 기준금액|
|include_regional_shipping_rate|지역별배송비 포함여부<br><br>T : 포함  <br>F : 미포함|
|shipments_start_date  <br><br>_최소값: [1]_  <br>_최대값: [30]_|배송시작일 설정|
|change_option|옵션 변경 가능 여부<br><br>T : 사용함  <br>F : 사용안함|

Update subscription products

> RequestcURL 

```java
MediaType mediaType = MediaType.parse("");
RequestBody body = RequestBody.create(mediaType, "{\n" +
"    \"shop_no\": 1,\n" +
"    \"request\": {\n" +
"        \"subscription_shipments_name\": \"SHIRTS SUBSCRIPTION SHIPMENTS MODIFY\",\n" +
"        \"product_binding_type\": \"P\",\n" +
"        \"one_time_purchase\": \"T\",\n" +
"        \"product_list\": [\n" +
"            11,\n" +
~~"            13\n" +
~~"        ],\n" +
"        \"use_discount\": \"T\",\n" +
"        \"discount_value_unit\": \"P\",\n" +
"        \"discount_values\": [\n" +
"            \"10.00\",\n" +
"            \"20.00\"\n" +
"        ],\n" +
"        \"subscription_shipments_cycle_type\": \"T\",\n" +
"        \"subscription_shipments_cycle\": [\n" +
"            \"3M\",\n" +
"            \"5M\"\n" +
"        ],\n" +
"        \"use_order_price_condition\": \"T\",\n" +
"        \"order_price_greater_than\": \"30000.00\",\n" +
"        \"include_regional_shipping_rate\": \"F\",\n" +
"        \"shipments_start_date\": 3,\n" +
"        \"change_option\": \"F\"\n" +
"    }\n" +
"}");
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/subscription/shipments/setting/72")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .put(body)
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "shipment": {
        "shop_no": 1,
        "subscription_no": 72,
        "subscription_shipments_name": "SHIRTS SUBSCRIPTION SHIPMENTS MODIFY",
        "product_binding_type": "P",
        "one_time_purchase": "T",
        "product_list": [
            11,
            13
        ],
        "use_discount": "T",
        "discount_value_unit": "P",
        "discount_values": [
            "10.00",
            "20.00"
        ],
        "subscription_shipments_cycle_type": "T",
        "subscription_shipments_cycle": [
            "3M",
            "5M"
        ],
        "use_order_price_condition": "T",
        "order_price_greater_than": "30000.00",
        "include_regional_shipping_rate": "F",
        "shipments_start_date": 3,
        "change_option": "F"
    }
}
```

### Delete subscription products 

DELETE/api/v2/admin/subscription/shipments/setting/{subscription_no}

설정된 정기배송 상품의 정기배송 설정을 해제할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 쓰기권한 (mall.write_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|**subscription_no**  <br>**Required**|정기배송 상품설정 번호|

Delete subscription products

> RequestcURL 

```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/subscription/shipments/setting/15")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .delete()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "shipment": {
        "shop_no": 1,
        "subscription_no": 15
    }
}
```

## Taxmanager

세금 관리자(MSA)의 활성화 정보 관련 기능입니다.

> Endpoints

```
GET /api/v2/admin/taxmanager
```

### Taxmanager properties

|**Attribute**|**Description**|
|---|---|
|use|세금 관리자 활성화 정보|

### Retrieve activation information for Tax Manager [](https://developers.cafe24.com/docs/ko/api/admin/#retrieve-activation-information-for-tax-manager)

GET/api/v2/admin/taxmanager

세금 관리자의 사용 정보를 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

Retrieve activation information for Tax Manager

> RequestcURL 

```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/taxmanager")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "taxmanager": {
        "use": "T"
    }
}
```

## Users

운영자(Users)는 쇼핑몰의 대표관리자와 더불어 쇼핑몰을 운영할 수 있는 운영자와 관련된 기능입니다.  
부운영자는 대표관리자가 부여한 권한 내에서 쇼핑몰을 운영할 수 있습니다.  
쇼핑몰에 등록된 운영자를 목록으로 조회하거나 특정 운영자를 조회할 수 있습니다.

> Endpoints

```
GET /api/v2/admin/users
GET /api/v2/admin/users/{user_id}
```

### Users properties

|**Attribute**|**Description**|
|---|---|
|user_id|운영자 아이디<br><br>운영자 혹은 부운영자의 아이디|
|user_name|운영자 명<br><br>운영자 혹은 부운영자의 이름|
|phone  <br><br>_전화번호_|전화번호<br><br>운영자 혹은 부운영자의 전화번호|
|email  <br><br>_이메일_|이메일<br><br>운영자 혹은 부운영자의 이메일 주소|
|ip_restriction_type|IP 접근제한<br><br>IP 접근제한의 사용여부<br><br>A : 사용함  <br>F : 사용안함|
|admin_type|운영자 구분<br><br>대표운영자인지 부운영자인지의 구분<br><br>P : 대표운영자  <br>A : 부운영자|
|last_login_date|최근 접속일시|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호|
|nick_name|운영자 별명<br><br>운영자의 별명|
|nick_name_icon_type|별명 아이콘 타입<br><br>별명 아이콘 등록. 직접 등록이나 샘플 등록이 가능함.<br><br>D : 직접 아이콘 등록  <br>S : 샘플 아이콘 등록|
|nick_name_icon_url|별명 아이콘 URL|
|board_exposure_setting|게시판 노출 설정|
|memo|메모|
|available|사용여부<br><br>T : 사용함  <br>F : 사용안함|
|multishop_access_authority|멀티쇼핑몰 접근 권한<br><br>T : 허용함  <br>F : 허용안함|
|menu_access_authority|메뉴 접근 권한|
|detail_authority_setting|상세 권한 설정|
|ip_access_restriction|IP 접근 제한|
|access_permission|접속 허용 권한<br><br>T : 접속 허용시간 설정과 상관없이 항상 관리자 페이지 접속을 허용함  <br>F : 사용안함|

### Retrieve a list of admin users 

GET/api/v2/admin/users

쇼핑몰에 등록된 운영자를 목록으로 조회할 수 있습니다.  
운영자 아이디, 운영자명, 이메일, 전화번호 등을 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|search_type|검색 타입<br><br>member_Id : 회원 아이디  <br>name : 이름|
|keyword|검색어|
|admin_type|운영자 구분<br><br>P : 대표운영자  <br>A : 부운영자|

Retrieve a list of admin users

> RequestcURL
```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/users")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "users": [
        {
            "user_id": "admin",
            "user_name": "John Doe",
            "phone": "02-0000-0000",
            "email": "sample@sample.com",
            "ip_restriction_type": "A",
            "admin_type": "P",
            "last_login_date": "2022-01-01T12:00:00+09:00"
        },
        {
            "user_id": "subadmin",
            "user_name": "Jane Doe",
            "phone": "02-0000-0000",
            "email": "sample@sample.com",
            "ip_restriction_type": "A",
            "admin_type": "A",
            "last_login_date": null
        }
    ]
}
```

### Retrieve admin user details 

GET/api/v2/admin/users/{user_id}

쇼핑몰에 등록된 특정 운영자를 조회할 수 있습니다.  
운영자 이름, 전화번호, 이메일 등을 조회할 수 있습니다.

#### 기본스펙

|**Property**|**Description**|
|---|---|
|SCOPE|**상점 읽기권한 (mall.read_store)**|
|호출건수 제한|**40**|

#### 요청사양

|**Parameter**|**Description**|
|---|---|
|shop_no  <br><br>_최소값: [1]_|멀티쇼핑몰 번호<br><br>DEFAULT 1|
|user_id|운영자 아이디<br><br>운영자 혹은 부운영자의 아이디|

Retrieve admin user details

> RequestcURL 

```java
Request request = new Request.Builder()
  .url("https://{mallid}.cafe24api.com/api/v2/admin/users/subadmin1")
  .addHeader("Authorization", "Bearer {access_token}")
  .addHeader("Content-Type", "application/json")
  .addHeader("X-Cafe24-Api-Version", "{version}")
  .get()
  .build();
  
OkHttpClient client = new OkHttpClient();
Response response = client.newCall(request).execute();
```

> Response

```json
{
    "user": {
        "shop_no": 1,
        "admin_type": "A",
        "user_name": "John Doe",
        "nick_name": "Cool John",
        "nick_name_icon_type": "S",
        "nick_name_icon_url": "https://img.echosting.cafe24.com/design/skin/admin/ko_KR/ico_nick1.gif",
        "board_exposure_setting": {
            "admin_grade_icon": "T",
            "nick_name_icon": "F",
            "writer_name_icon": "F"
        },
        "phone": "02-1234-5678",
        "email": "subadmin1@cafe24.com",
        "memo": "test memo",
        "available": "T",
        "multishop_access_authority": "T",
        "menu_access_authority": {
            "order": {
                "authority": true,
                "detail_setting": {
                    "74": {
                        "key": "74",
                        "name": "전체 주문 조회",
                        "authority": true
                    },
                    "71": {
                        "key": "71",
                        "name": "입금전 관리",
                        "authority": true
                    },
                    "72": {
                        "key": "72",
                        "name": "배송 준비중 관리",
                        "authority": true
                    }
                }
            },
            "product": {
                "authority": true,
                "detail_setting": {
                    "2037": {
                        "key": "2037",
                        "name": "상품목록",
                        "authority": true
                    },
                    "2031": {
                        "key": "2031",
                        "name": "상품등록",
                        "authority": true,
                        "children": {
                            "2032": {
                                "key": "2032",
                                "name": "간단 등록",
                                "authority": true
                            },
                            "2033": {
                                "key": "2033",
                                "name": "일반 등록",
                                "authority": true
                            },
                            "2138": {
                                "key": "2138",
                                "name": "세트 상품 등록",
                                "authority": true
                            },
                            "2135": {
                                "key": "2135",
                                "name": "엑셀 등록",
                                "authority": true
                            }
                        }
                    }
                }
            }
        },
        "detail_authority_setting": {
            "product": {
                "edit_product_category": "F",
                "modify_product_info": null,
                "remove_product_info": "F",
                "change_product_sale_status": "F",
                "change_product_display_status": "F",
                "edit_product_display_reservation": "F",
                "download_product_excel_data_in_menu": "F",
                "show_product_supply_business": "F",
                "edit_product_supply_business": "F",
                "edit_product_supply_production_cost": "F",
                "show_supplier_product_name": "F",
                "edit_product_manufacturer_info": "F",
                "show_product_delivery_count": "F",
                "show_product_sales_count": "F"
            },
            "order": {
                "restrict_searching_order_info": "F",
                "restrict_searching_personal_info": "F",
                "restrict_printing_in_menu": "F",
                "check_payment": "F",
                "cancel_credit_payment": "F",
                "cancel_payco_point_payment": "F",
                "cancel_affiliated_gift_certificate_payment": "F",
                "cancel_affiliation_point_payment": "F",
                "cancel_order": "F",
                "return_product": "F",
                "exchange_product": "F",
                "accept_refunding_product": "F",
                "handle_refunding_product": "F",
                "edit_order_memo": "F",
                "download_order_data_in_menu": "F",
                "show_dashboard_order_info": "F",
                "show_total_ordered_amount": "F",
                "show_integration_balance": "F",
                "cancel_withdrawal": "F",
                "exchange_withdrawal": "F",
                "return_withdrawal": "F",
                "refund_withdrawal": "F"
            }
        },
        "ip_access_restriction": {
            "usage": "T",
            "registered_ip_list": [
                "127.0.0.1"
            ]
        },
        "access_permission": "F"
    }
}
```

