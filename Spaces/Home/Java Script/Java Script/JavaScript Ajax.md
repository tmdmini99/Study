



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

