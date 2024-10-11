

`@RestController`를 사용하면, `@Controller`와 `@ResponseBody`를 결합한 것과 같은 역할을 합니다. 즉, `@RestController`는 해당 클래스에 있는 모든 메서드가 **기본적으로 JSON 형식의 데이터를 반환**하도록 동작합니다.

### `@RestController`를 사용하는 경우

- **API에서 JSON이나 XML 데이터를 반환할 때**: AJAX 요청을 처리하여 데이터를 반환하거나 RESTful API를 만들 때 적합합니다.
- 모든 메서드가 데이터를 반환하고, 템플릿(JSP 등)을 렌더링할 필요가 없을 때 사용합니다.

```java
@RestController
@RequestMapping("/customers/*")
@RequiredArgsConstructor
public class CustomersController {

    private final CustomersService customersService;

    // JSON 데이터를 반환하는 메서드
    @GetMapping("customer-info")
    public CustomerVo getCustomerInfo(@RequestParam("id") String customerId) {
        return customersService.getCustomerInfo(customerId); // JSON 형식으로 반환
    }
}

```


방식으로 `@RestController`를 사용하면 모든 메서드가 **자동으로 JSON 형식**으로 데이터를 반환하게 됩니다. 따라서 별도로 `@ResponseBody`를 메서드마다 붙일 필요가 없습니다.

### `@RestController`가 적합하지 않은 경우

- **템플릿 파일(JSP, Thymeleaf 등)을 렌더링해야 할 때**: `@RestController`는 JSP 파일이나 HTML 파일을 반환하지 않습니다. 템플릿 렌더링이 필요하면 `@Controller`를 사용해야 합니다.

예를 들어, 아래처럼 JSP 파일을 반환하려고 하면, `@RestController`는 텍스트 데이터를 그대로 반환하기 때문에 의도한 동작이 이루어지지 않습니다.



```java
@RestController
@RequestMapping("/customers/*")
@RequiredArgsConstructor
public class CustomersController {

    private final CustomersService customersService;

    // 이 메서드는 JSP 파일을 반환해야 하지만 RestController에서는 동작하지 않음
    @GetMapping("customer")
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {
        List<OrdersVo> list = customersService.selectList(basicParamVo);
        model.addAttribute("list", list);
        return "customers/customers"; // JSP 페이지를 반환하고 싶지만 RestController에서는 작동하지 않음
    }
}

```


- **JSON 데이터만 반환해야 하는 경우**라면 `@RestController`가 편리하고 적합합니다.
- **JSP 같은 템플릿을 렌더링해야 하는 경우**에는 `@Controller`를 사용하고, JSON 응답이 필요한 메서드에만 `@ResponseBody`를 사용해야 합니다.

따라서 **JSP와 JSON 응답을 둘 다 사용하려면** `@Controller`를 사용하고, 특정 메서드에만 `@ResponseBody`를 적용하는 방식이 더 적합합니다.