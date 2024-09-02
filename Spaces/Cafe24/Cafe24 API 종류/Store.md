
## Store API

![[cafe25 Store1.png]]


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


![[Pasted image 20240902101532.png]]


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

### Automessages arguments properties [](https://developers.cafe24.com/docs/ko/api/admin/#automessages-arguments-properties)

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

  ![[Pasted image 20240830175815.png]]
  
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

  ![[Pasted image 20240830175945.png]]
  
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

더보기