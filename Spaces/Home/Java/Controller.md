

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




```java
@GetMapping("orders/{id}")
public String ordersList(@PathVariable("id") Long id, Model model) {
    // 여기서 id는 URL에서 가져온 값입니다.
    List<OrdersVo> list = ordersService.selectList(id);
    model.addAttribute("list", list);
    return "orders/orders";
}

```


 `@GetMapping("orders/{id}")`는 URL 경로에서 동적으로 값을 받을 수 있도록 설정한 것입니다. 이 구조에서 `{id}`는 URL의 경로 변수로, 클라이언트가 요청하는 URL의 특정 부분에 해당하는 값을 자동으로 메서드의 파라미터로 매핑합니다.


`@PathVariable`은 Spring MVC에서 URL 경로의 일부를 메서드 파라미터로 전달할 수 있게 해주는 애너테이션입니다. 이 애너테이션을 사용하면 클라이언트가 보낸 요청의 URL에서 특정 값을 추출하여 사용할 수 있습니다.


- `@PathVariable("id") Long id`: `@PathVariable` 애너테이션을 사용하여 URL의 `{id}` 부분을 `id`라는 이름의 메서드 파라미터로 받습니다. 이 값은 URL 요청에 따라 다르게 들어오며, 이 메서드에서는 주문 ID를 나타냅니다.
- 예를 들어, `GET /orders/123` 요청이 들어오면 `id`는 `123`이 되고, 이 값을 사용하여 주문 목록을 조회합니다.




`@RequestMapping("/orders/*")`와 `@RequestMapping("/orders")`는 비슷한 점도 있지만, 주요한 차이점이 있습니다. 이 두 매핑은 요청을 처리하는 방식이 다릅니다. 아래에서 각각의 의미와 차이점을 설명하겠습니다.

### 1. `@RequestMapping("/orders/*")`

- **의미**: 이 매핑은 `/orders`로 시작하는 모든 경로에 대해 해당 컨트롤러의 메서드를 처리합니다.
- **예시**:
    - `/orders` (리스트 페이지)
    - `/orders/123` (주문 상세 페이지)
    - `/orders/abc` (다른 주문 상세 페이지)
- **장점**: 한 컨트롤러에서 `/orders`로 시작하는 모든 요청을 쉽게 처리할 수 있습니다.
- **단점**: 모든 경로를 처리하는 경향이 있어, 특정 경로에 대해 세부적인 처리가 필요할 경우 관리가 복잡해질 수 있습니다.

### 2. `@RequestMapping("/orders")`

- **의미**: 이 매핑은 `/orders` 경로만 처리합니다. 그 아래에 세부적인 메서드에서 추가 매핑을 설정해야 합니다.
- **예시**:
    - `/orders` (리스트 페이지) → `@GetMapping("")` 또는 `@GetMapping("/")`
    - `/orders/{id}` (주문 상세 페이지) → `@GetMapping("/{id}")`
- **장점**: 더 명확하고 유지 관리하기 쉽습니다. 각 요청에 대해 구체적으로 매핑할 수 있어, 나중에 수정하기도 용이합니다.
- **단점**: 한 컨트롤러가 처리하는 경로가 많아질 경우, 코드가 길어질 수 있습니다.