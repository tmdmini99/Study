
## Store API

![[Pasted image 20240830100055.png]]


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



