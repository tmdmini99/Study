

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

url 업데이트 후 뒤로가기 시 필요

```js
// popstate 이벤트 리스너 추가  
    $(window).on('popstate', function(event) {  
        if (event.originalEvent.state) {  
            const id = event.originalEvent.state.id; // 저장된 ID 가져오기  
            $.ajax({  
                url: '/orders/' + id, // 해당 ID로 서버에 요청  
                method: 'GET',  
                success: function(response) {  
                    $('body').html(response); // <body> 내용을 업데이트  
                },  
                error: function(xhr, status, error) {  
                    console.error('AJAX 요청 실패:', error);  
                }  
            });  
        } else {  
            // 상태가 없을 경우 기본 페이지로 리로드  
            window.location.reload(); // 원하시면 초기화면으로 리로드  
        }  
    });
```




동적으로 변환

```js
$(document).ready(function() {  
    $.ajaxSetup({ cache: false });  
  
    // 이벤트 위임을 사용하여 ul 또는 li에 클릭 이벤트를 등록  
    $('ul').on('click', 'li', function(event) {  
        event.preventDefault(); // 기본 링크 클릭 이벤트 방지  
        console.log('리스트 아이템 클릭됨'); // 클릭 이벤트 확인 로그  
  
        // hidden input에서 id 가져오기  
        const id = $(this).closest('li').prev("input[type='hidden']").val(); // li의 이전 노드 중 hidden input의 값 가져오기  
        console.log('Hidden ID:', id); // hidden input에서 가져온 id 출력  
  
        // 현재 주소를 가져옴  
        const currentUrl = window.location.href;  
  
        $.ajax({  
            url: currentUrl+ '/' + id, // 현재 주소로 요청을 보냄  
            method: 'GET',  
            success: function(response) {  
                console.log('AJAX 요청 성공:', response); // 성공 시 로그 출력  
                // 서버에서 응답받은 HTML을 결과 영역에 삽입  
                $('body').html(response);  
  
                // URL을 업데이트 (history.pushState 사용)  
                const newUrl = currentUrl+"/"+id;  
                history.pushState({ id: id }, '', newUrl);  
  
            },  
            error: function(xhr, status, error) {  
                console.error('AJAX 요청 실패:', error); // 에러 로그 출력  
            }  
        });  
    });  
  
    // popstate 이벤트 리스너 추가  
    $(window).on('popstate', function(event) {  
        if (event.originalEvent.state) {  
            const id = event.originalEvent.state.id; // 저장된 ID 가져오기  
            $.ajax({  
                url: window.location.href, // 현재 URL로 요청  
                method: 'GET',  
                success: function(response) {  
                    $('body').html(response); // <body> 내용을 업데이트  
                },  
                error: function(xhr, status, error) {  
                    console.error('AJAX 요청 실패:', error);  
                }  
            });  
        } else {  
            // 상태가 없을 경우 기본 페이지로 리로드  
            window.location.reload(); // 원하시면 초기화면으로 리로드  
        }  
    });  
});
```