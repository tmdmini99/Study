### Spring MVC에서의 `Model` 객체

1. **메서드 매개변수**:
    
    - `@GetMapping` 같은 메서드에 `Model` 매개변수를 선언하면, Spring은 해당 메서드가 호출될 때 자동으로 `Model` 객체를 생성하여 전달합니다.


```java
@GetMapping("payments/{id}") public String ordersList(@PathVariable("id") String id, @ModelAttribute BasicParamVo basicParamVo, Model model) { // 결제 트랜잭션 목록을 선택합니다. List<PaymentsTransactionVo> paymentsVo = (List<PaymentsTransactionVo>) paymentsService.selectTransaction(basicParamVo); // model에 결제 목록을 추가합니다. model.addAttribute("list", paymentsVo); // 뷰 이름을 반환합니다. return "payments/paymentsDetail"; }
```