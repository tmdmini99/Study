
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