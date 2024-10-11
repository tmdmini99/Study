



```js
document.addEventListener("DOMContentLoaded", function() {
    // 버튼 클릭 이벤트 리스너
    document.querySelectorAll('.load-customer-data').forEach(button => {
        button.addEventListener('click', function() {
            const customerId = this.getAttribute('data-id');

            // AJAX 요청
            fetch(`/api/customers/${customerId}`) // API 엔드포인트는 적절히 수정해야 합니다.
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    // 응답받은 데이터로 스팬 업데이트
                    const row = document.getElementById(`customer-${customerId}`);
                    if (row) {
                        row.querySelector('.customer-name').textContent = data.name || '이름 없음';
                        row.querySelector('.customer-location').textContent = data.location || '위치 없음';
                        row.querySelector('.customer-email').textContent = data.email || '이메일 없음';
                    }
                })
                .catch(error => {
                    console.error('There was a problem with the fetch operation:', error);
                });
        });
    });
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