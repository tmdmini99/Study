
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