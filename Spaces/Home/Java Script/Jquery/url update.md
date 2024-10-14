

ajax로 페이지 로드 후 url 변경
```js
$('tbody tr').on('click', function() {  
    const id = $(this).attr('id'); // 클릭한 tr의 id 가져오기  
  
    $.ajax({  
        url: '/orders/orders/' + id, // AJAX 요청을 보낼 URL에 id를 붙임  
        method: 'GET',  
        success: function(response) {  
            // 서버에서 응답받은 HTML을 결과 영역에 삽입  
            $('#result').html(response);  
  
            // URL을 업데이트 (history.pushState 사용)  
            const newUrl = window.location.protocol + "//" + window.location.host + '/orders/orders/' + id;  
            history.pushState({ id: id }, '', newUrl);  
        },  
        error: function(xhr, status, error) {  
            console.error('AJAX 요청 실패:', error);  
        }  
    });  
});
```

jsp로 페이지 변경후 
html로 전부 변경
비동기로 페이지 변경