
```javaScript
$.ajax({
  url: "/some-url/api", // 요청 URL
  type: "GET", // HTTP 메서드
  dataType: "json", // 응답으로 예상되는 데이터 타입
  success: function(data) {
    // 요청 성공 시 실행될 함수
    console.log("Response:", data);
  },
  error: function(xhr, status, error) {
    // 요청 실패 시 실행될 함수
    console.error("Request failed:", error);
  }
});

```


## JSON 데이터 가져오기

```javaScript
$.getJSON("/some-url/api", function(data) {
  console.log("Response:", data);
});

```


## requestAjax

```javaScript
//일반적인 jquery ajax
function requestAjax(url, params, callback) {
    $.ajax({
        url: url,
        type: "POST",
        contentType: "application/json",
        data: params,
        success: callback,
        error: function(xhr, status, error) {
            console.error("Request failed:", error);
        }
    });
}

// 사용 예시 좀더 간단히 사용한 ajax
var params = {
    key1: "value1",
    key2: "value2"
};

requestAjax("/route/01130cud.bms", JSON.stringify(params), function(data) {
    console.log("Response:", data);
});




$(document).ready(function() {
            // 버튼 클릭 이벤트 리스너
            $('.load-customer-data').on('click', function() {
                const customerId = $(this).data('id'); // 버튼에서 customer ID 가져오기

                // AJAX 요청
                $.ajax({  
				    url: `/customers/customers`, // API 엔드포인트  
				    method: 'GET',  
				    data: { id: customerId }, // 데이터로 고객 ID 전송  
				    dataType: 'json',  
				    success: function(data) {  
				        // 응답받은 데이터로 고객 정보 업데이트  
				        const row = $('.custom-Popover__Section');  
				        row.find('.customer-name').text(data.name || '이름 없음');  
				        row.find('.customer-location').text(data.location || '위치 없음');  
				        row.find('.customer-order-count').text(`주문 ${data.orderCount || 0}개`);  
				        row.find('.customer-email').text(data.email || '이메일 없음');  
				  
				        row.find('.customer').closest('a').attr('href', `/customers/${customerId}`); // href 속성 변경  
				        row.find('.custom-Popover__Section').toggle(); // 클릭 시 div를 보이거나 숨김  
				    },  
				    error: function(xhr, status, error) {  
				        console.error('AJAX 요청 중 오류 발생:', error);  
				        alert('고객 정보를 가져오는 중 오류가 발생했습니다.'); // 사용자에게 오류 알림  
				    }  
				});
            });
        });

```


동적이라 이벤트를 동적으로 줘야함
```js
$(document).on('click', '.icon', function() {  
const icon = $(this); // 클릭한 아이콘  
const customerId = icon.closest('tr').attr('id'); // ID에서 고객 ID 가져오기  
console.log('고객 ID:', customerId);  
  
// AJAX 요청  
$.ajax({  
    url: '/customers/customer',  
    method: 'GET',  
    data: { id: customerId },  
    dataType: 'json',  
    success: function(data) {  
        // 응답받은 데이터로 고객 정보 업데이트  
        const popover = $('.custom-Popover__Section'); // 팝오버 요소  
        const fullName = (data.firstName || '') + ' ' + (data.lastName || '');  // 이름이 없을 경우 공백 처리  
        popover.find('.customer-name').text(fullName || '이름 없음');  
        popover.find('.customer-location').text(data.defaultAddress['city'] || '위치 없음');  
        popover.find('.customer-order-count').text('주문 ' + (data.orderCount || 0) + '개');  
        popover.find('.customer-email').text(data.email || '이메일 없음');  
  
        popover.find('.customer').closest('a').attr('href', `/customers/customers/${customerId}`); // href 속성 변경  
  
        // 클릭한 아이콘 위치 계산  
        const iconOffset = icon.offset(); // 클릭한 아이콘의 위치  
        const iconHeight = icon.outerHeight(); // 아이콘의 높이  
  
        // 팝오버의 위치를 아이콘 바로 아래로 설정  
        popover.css({  
            top: iconOffset.top + iconHeight + 5, // 아이콘 아래로 위치  
            left: iconOffset.left // 아이콘과 같은 수평 위치  
        });  
  
        // 토글로 말풍선 보이기/숨기기  
        popover.toggle(); // 말풍선 보이기 또는 숨기기  
    },  
    error: function(xhr, status, error) {  
        console.error('AJAX 요청 중 오류 발생:', error);  
        alert('고객 정보를 가져오는 중 오류가 발생했습니다.'); // 사용자에게 오류 알림  
    }  
});
```



### **1. jQuery의 `filter()`**

#### **용도**:

jQuery의 `filter()`는 **jQuery 객체**에서 선택된 DOM 요소를 필터링하는 데 사용됩니다. 특정 조건에 맞는 요소만 유지합니다.

```js
$(selector).filter(selector/function)
```

#### **인수**:

1. **selector**: 필터링할 기준이 되는 CSS 선택자.
2. **function(index, element)**: 각 요소에 대해 실행되는 함수.
    - **`index`**: 요소의 인덱스.
    - **`element`**: 현재 요소.

#### **예제**:

```js
// 1. 특정 클래스만 필터링
$('.items').filter('.active').css('color', 'red');

// 2. 조건 기반 필터링
$('.items').filter(function(index, element) {
    return $(element).text() === '특정 텍스트';
}).css('color', 'blue');
```


#### **특징**:

- jQuery `filter()`는 jQuery 객체를 반환합니다. 따라서, 체이닝이 가능합니다.
- DOM 요소의 스타일이나 속성을 쉽게 변경할 수 있습니다.


filter 거는법
```js
$('.btc').filter(function() {  
    return !$(this).parent().hasClass('category-manage-popup');  
}).prepend
```



### **3. 주요 차이점**

|**특징**|**jQuery `filter()`**|**JavaScript `filter()`**|
|---|---|---|
|**대상**|jQuery 객체|배열|
|**결과 타입**|jQuery 객체|새 배열|
|**인수**|CSS 선택자 또는 함수|콜백 함수|
|**체이닝 지원**|가능|불가능|
|**DOM 요소 필터링**|기본적으로 DOM 요소를 대상으로 동작|NodeList를 배열로 변환해야 사용 가능|


### **4. jQuery와 JavaScript의 `filter`를 활용한 동일 작업**

#### **jQuery**:

```js
$('.items').filter('.active').css('color', 'red');
```


**JavaScript**:

```js
Array.from(document.querySelectorAll('.items'))
    .filter(function(el) {
        return el.classList.contains('active');
    })
    .forEach(function(el) {
        el.style.color = 'red';
    });
```


- **DOM 요소를 필터링하고 체이닝을 원한다면**: jQuery의 `filter()`.
- **배열 데이터를 필터링하거나 DOM 조작이 간단하다면**: JavaScript의 `filter()`.