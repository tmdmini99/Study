

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
	                let newDocument = new DOMParser().parseFromString(response, 'text/html');  
	                let newContent = newDocument.getElementById('content'); // 업데이트할 부분 선택  
	  
	                // 기존 콘텐츠 업데이트  
	                if (newContent) {  
	                       document.getElementById('content').innerHTML = newContent.innerHTML; // 기존 content만 업데이트  
	                }  
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


### 코드 분석:

1. **`$(window).on('popstate', function(event) {...});`**:
    
    - `popstate` 이벤트는 브라우저의 히스토리 상태가 변경될 때(뒤로 가기나 앞으로 가기 버튼을 클릭했을 때) 발생합니다.
    - 이 코드는 `popstate` 이벤트가 발생하면 `event.originalEvent.state`를 검사하여 히스토리에 저장된 상태 객체가 있는지 확인합니다.
2. **`if (event.originalEvent.state)`**:
    
    - `event.originalEvent.state`는 브라우저 히스토리에서 저장된 상태를 가져옵니다. 예를 들어, 페이지를 이동할 때 `history.pushState()`나 `history.replaceState()`로 저장된 상태 정보입니다.
    - 이 조건문은 히스토리 상태에 값이 있을 경우에만 실행되며, 상태가 없을 경우(`else` 블록)가 처리됩니다.
3. **`const id = event.originalEvent.state.id;`**:
    
    - 이 라인은 `popstate` 이벤트에 포함된 상태에서 `id` 값을 가져옵니다. 이 `id` 값은 브라우저 히스토리에 저장된 특정 정보를 식별하는 데 사용됩니다.
4. **AJAX 요청 (`$.ajax({...})`)**:
    
    - `url: window.location.href`: 현재 페이지의 URL로 AJAX 요청을 보냅니다. 현재 페이지를 다시 서버에 요청하는 동작입니다.
    - `method: 'GET'`: GET 방식으로 요청을 보내서 페이지의 내용을 받아옵니다.
    - `success: function(response) {...}`: 요청이 성공하면, 서버에서 받은 응답(`response`)을 처리합니다. 이 응답은 페이지의 전체 HTML이 될 것입니다.
5. **`let newDocument = new DOMParser().parseFromString(response, 'text/html');`**:
    
    - 이 줄은 서버에서 받은 HTML 응답을 **DOM 형식**으로 변환합니다.
    - `DOMParser`는 문자열로 된 HTML 코드를 파싱하여 문서 객체로 변환합니다.
6. **`let newContent = newDocument.getElementById('content');`**:
    
    - 변환된 문서에서 `<div id="content">` 요소를 선택합니다. 이는 페이지에서 동적으로 교체할 부분을 선택하는 단계입니다.
7. **`document.getElementById('content').innerHTML = newContent.innerHTML;`**:
    
    - 현재 페이지에서 `<div id="content">` 요소의 내용을 서버에서 받은 새 콘텐츠로 교체합니다. 이렇게 하면 페이지 전체를 새로고침하지 않고 필요한 부분만 동적으로 업데이트할 수 있습니다.
8. **`else { window.location.reload(); }`**:
    
    - 만약 `popstate` 이벤트에 저장된 상태가 없다면, 기본 동작으로 페이지 전체를 새로고침합니다.
    - 이 코드는 브라우저 히스토리에 상태 정보가 없을 때, 예를 들어 사용자가 처음 페이지를 로드했을 때 발생할 수 있습니다.