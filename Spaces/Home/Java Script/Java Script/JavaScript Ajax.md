



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



