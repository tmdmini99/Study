



```js
document.addEventListener("DOMContentLoaded", function() {
    // 버튼 클릭 이벤트 리스너
    document.querySelectorAll('.load-customer-data').forEach(button => {
        button.addEventListener('click', function() {
            const customerId = this.getAttribute('data-id');

            // AJAX 요청
            $.ajax({
				url: "https://jsonplaceholder.typicode.com/posts/1", // 요청 보낼 URL
				method: "GET",  // GET 요청 방식
				success: function(response) {
					// 성공적으로 데이터를 받아온 경우
					$("#result").html(`
						<h3>${response.title}</h3>
						<p>${response.body}</p>
					`);
				},
				error: function(error) {
					// 에러 발생 시
					$("#result").html("<p>Something went wrong.</p>");
				}
			});
        });
    });
});

```

```js
$.ajax({
	url: "https://jsonplaceholder.typicode.com/posts",  // POST 요청을 보낼 URL
	method: "POST", // POST 방식
	data: JSON.stringify(postData), // 데이터를 JSON 형식으로 변환
	contentType: "application/json; charset=UTF-8", // 전송할 데이터의 형식 지정
	success: function(response) {
		// 성공적으로 데이터를 보낸 경우 응답 데이터 출력
		$("#responseResult").html(`
			<h3>Post Created Successfully</h3>
			<p><strong>ID:</strong> ${response.id}</p>
			<p><strong>Title:</strong> ${response.title}</p>
			<p><strong>Body:</strong> ${response.body}</p>
		`);
	},
	error: function(error) {
		// 에러 발생 시
		$("#responseResult").html("<p>Something went wrong.</p>");
	}
});
```




이런식으로 메소드 밑에 @ResponseBody 사용
```java
  
@Controller  
@RequestMapping("/customers/*")  
@RequiredArgsConstructor  
public class CustomersController {  
    private final CustomersService customersService;  
  
    @GetMapping("customer")  
    @ResponseBody  
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {  
  
        List<OrdersVo> list = customersService.selectList(basicParamVo);  
        model.addAttribute("list", list);  
        return "customers/customers";  
    }  
}
```


이렇게 사용시 controller 모든 메소드에 responseBody 적용

```java
@Controller
@RequestMapping("/customers/*")
@RequiredArgsConstructor
@ResponseBody
public class CustomersController {
    private final CustomersService customersService;

    @GetMapping("customer")
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {

        List<OrdersVo> list = customersService.selectList(basicParamVo);
        model.addAttribute("list", list);
        return "customers/customers";
    }
} 
```



 
## 동적으로 실행



**동적으로 페이지를 이동**하고 HTML 콘텐츠를 교체하는 경우, 특히 **AJAX**로 요청한 **JSP 페이지**의 스크립트는 자동으로 실행되지 않습니다. 이는 보안상의 이유로, 동적으로 로드된 HTML 내에 포함된 `<script>` 태그가 **자동으로 실행되지 않도록** 되어 있기 때문입니다.

### 이유:

`innerHTML`을 사용하여 HTML을 삽입하거나 `fetch` API로 HTML을 로드하는 경우, 로드된 HTML의 **JavaScript 코드**는 **자동으로 실행되지 않습니다**. 이는 **브라우저의 보안 정책**에 따른 것입니다. `<script>` 태그가 포함된 HTML을 `innerHTML`로 삽입한다고 해서 그 안의 JavaScript 코드가 실행되는 것이 아니라, **HTML 구조만 삽입**됩니다.

### 해결책:

동적으로 페이지를 변경한 후에도 해당 페이지 내의 **JSP에서 포함된 JavaScript 코드**를 실행하려면, **스크립트를 명시적으로 실행**하거나 **동적으로 `<script>` 태그를 추가**해야 합니다.

### 방법:

1. **`innerHTML`로 HTML 삽입 후 스크립트 실행**
    
    - 새로운 HTML 콘텐츠에 포함된 `<script>` 태그를 **수동으로 실행**하거나 **동적으로 추가**해야 합니다.
2. **동적으로 `<script>` 태그 삽입하여 실행**
    

다음은 동적으로 HTML 콘텐츠를 로드한 후 **스크립트**를 실행하는 방법입니다.

#### 방법 1: 동적으로 `<script>` 태그 삽입


```js
// AJAX로 HTML 콘텐츠를 받아서 페이지에 적용
fetch(url, {
    method: 'GET',
    headers: {
        'Content-Type': 'text/html'
    }
})
.then(response => response.text())
.then(response => {
    // 서버에서 받은 HTML을 DOMParser로 파싱
    let newDocument = new DOMParser().parseFromString(response, 'text/html');
    let newContent = newDocument.getElementById('content'); // 업데이트할 부분 선택

    // 기존 content만 업데이트
    if (newContent) {
        document.getElementById('content').innerHTML = newContent.innerHTML;
        
        // 새로운 HTML 내의 <script> 태그를 찾아서 실행
        let scripts = newDocument.querySelectorAll('script');
        scripts.forEach(script => {
            let newScript = document.createElement('script');
            newScript.textContent = script.textContent; // 새로운 스크립트 내용
            document.body.appendChild(newScript); // 동적으로 <script> 태그를 body에 추가하여 실행
        });
    }
    renderIcons(); // 아이콘 다시 렌더링
    history.pushState({ id: url }, '', url); // URL 업데이트
})
.catch(error => {
    console.error('AJAX 요청 실패:', error);
});

```


#### 방법 2: 외부 스크립트 파일 로드

만약 동적으로 로드한 페이지가 **외부 스크립트**를 포함하고 있다면, 그 스크립트를 동적으로 로드할 수 있습니다.



```js
let scripts = newDocument.querySelectorAll('script');
scripts.forEach(script => {
    if (script.src) {
        let scriptTag = document.createElement('script');
        scriptTag.src = script.src; // 외부 스크립트의 src 속성
        document.body.appendChild(scriptTag); // 동적으로 외부 스크립트 로드
    }
});
```


### JSP 페이지에서 JavaScript 실행:

JSP 파일 내에서 JavaScript를 사용하는 경우에도, **AJAX로 JSP 파일을 로드**하고 그 콘텐츠를 페이지에 삽입한 후에는 **해당 스크립트가 실행되지 않기** 때문에, 위와 같은 방법으로 **동적으로 `<script>` 태그를 삽입**하거나 **스크립트를 실행**해야 합니다.

### 결론:

- **AJAX**나 **`fetch`**로 JSP 페이지를 로드한 경우, **스크립트**는 자동으로 실행되지 않기 때문에 **수동으로 실행**하거나 **동적으로 `<script>` 태그를 추가**하여 스크립트를 실행해야 합니다.
- `<script>` 태그를 동적으로 추가하는 방법이나 외부 스크립트를 동적으로 로드하는 방법을 활용하면, 페이지의 스크립트가 정상적으로 실행됩니다.



```js
$(document).ready(function() {  
  
    // 테이블에서 행 클릭 시 fetch로 데이터 요청  
    $("table tbody tr[data-id]").on("click", function() {  
        var rowId = $(this).data("id");  
        var currentUrl = window.location.pathname;  // 예: /board/3300  
        var detailUrl = currentUrl + 'Detail';  // 예: /board/3300Detail  
  
        // fetch로 AJAX 요청을 보내는 부분  
        $.ajax({  
            url: detailUrl,  // 요청 URL            method: 'GET',  // GET 방식으로 요청  
            data: {  
                sc_id: rowId  // 파라미터로 sc_id 전송  
            },  
            headers: {  
                'X-Requested-With': 'XMLHttpRequest',  // AJAX 요청을 알리기 위한 헤더  
                'Content-Type': 'application/json'     // 요청 본문 데이터 형식을 JSON으로 설정  
            },  
            success: function(data) {  
                console.log(data);  // 서버에서 받은 데이터 확인  
  
                var tbody = $('table tbody[data-detail]');  // tbody 요소 선택  
                console.log(tbody);  // 서버에서 받은 데이터 확인  
  
                var newDocument = $.parseHTML(data);  // 응답 데이터를 HTML로 파싱  
                var newContent = $(newDocument).find('table tbody[data-detail]');  // 파싱한 HTML에서 원하는 요소 찾기  
                console.log(newContent);  
  
                tbody.html(newContent.html());  // 기존 tbody의 내용을 새로 받은 데이터로 갱신  
  
                var sc_id = $('#sc_id');  // sc_id 요소 선택  
                console.log(sc_id);  
  
                // sc_id 값 설정  
                sc_id.val($(newDocument).find('#sc_id').val());  
                var actionUrl = $('#search-form').attr('action');  
                // var form = $('<form></form>');  // 새로운 form 태그 생성  
                // form.attr('method', 'POST');  // POST 방식으로 설정  
                // form.attr('action', actionUrl);  // 적절한 action URL을 설정  
                //  
                // // sc_id를 hidden 필드로 추가  
                // var scIdInput = $('<input>').attr('type', 'hidden').attr('name', 'sc_id').val(sc_id.val());  
                // form.append(scIdInput);                // var csrf = $('<input>').attr('type', 'hidden').attr('name', $('#csrf').attr('name')).val($('#csrf').val());                // form.append(csrf);                // form.css('display', 'none');  // 폼을 숨김  
                // $('body').append(form);  
                // form.submit();  // 폼 제출  
                // form.remove(); // 폼 삭제  
                // form 데이터를 객체 형태로 준비  
                var formData = {  
                    '_csrf': $('#csrf').val(),  // CSRF 토큰 값  
                    'sc_id': sc_id.val()  // sc_id 값  
                };  
  
                // AJAX로 폼 데이터를 POST 방식으로 서버에 비동기적으로 전송  
                $.ajax({  
                    url: actionUrl,  // 폼의 action URL 사용  
                    method: 'POST',  
                    data: formData,  // 폼 데이터를 전송  
                    success: function(response) {  
                        // 서버로부터 성공적인 응답을 받았을 때 처리  
                        console.log('폼 데이터 전송 성공:', response);  
                    },  
                    error: function(xhr, status, error) {  
                        // 요청 실패 시 오류 처리  
                        console.error("AJAX error:", error);  
                    }  
                });  
            },  
            error: function(xhr, status, error) {  
                // 요청 실패 시 오류 처리  
                console.error("AJAX error:", error);  
            }  
        });  
  
});  
});
```
