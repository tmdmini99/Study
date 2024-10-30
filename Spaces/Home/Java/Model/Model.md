### Spring MVC에서의 `Model` 객체

1. **메서드 매개변수**:
    
    - `@GetMapping` 같은 메서드에 `Model` 매개변수를 선언하면, Spring은 해당 메서드가 호출될 때 자동으로 `Model` 객체를 생성하여 전달합니다.


```java
@GetMapping("payments/{id}")
public String ordersList(@PathVariable("id") String id, @ModelAttribute BasicParamVo basicParamVo, Model model) {
    // 결제 트랜잭션 목록을 선택합니다.
    List<PaymentsTransactionVo> paymentsVo = (List<PaymentsTransactionVo>) paymentsService.selectTransaction(basicParamVo);
    
    // model에 결제 목록을 추가합니다.
    model.addAttribute("list", paymentsVo);
    
    // 뷰 이름을 반환합니다.
    return "payments/paymentsDetail";
}
```

### 작동 원리

- **Spring의 DI(Dependency Injection)**:
    - Spring은 `Model` 객체를 메서드의 매개변수로 자동으로 주입합니다. 개발자는 `new Model()`과 같은 방식으로 객체를 생성할 필요가 없습니다.
- **전달된 데이터 사용**:
    - 이후에 지정된 뷰(`payments/paymentsDetail`)에서 `model`에 추가한 데이터를 사용할 수 있습니다.

### 왜 서비스에서 `Model`을 사용하지 않는가?

1. **관심 분리(Separation of Concerns)**:
    
    - 서비스 레이어는 비즈니스 로직을 처리하는 곳이고, 컨트롤러 레이어는 요청을 처리하고 응답을 준비하는 곳입니다. `Model`은 뷰와 관련된 정보를 담고 있어야 하므로, 이 두 레이어의 책임을 명확히 분리하는 것이 좋습니다.
2. **재사용성**:
    
    - 서비스가 `Model`을 직접 다루면, 해당 서비스는 특정한 MVC 프레임워크에 종속적이게 됩니다. 반면에, 서비스 레이어는 비즈니스 로직을 포함하고 있으며, 다른 애플리케이션이나 다른 레이어에서도 재사용될 수 있어야 합니다.
3. **단위 테스트**:
    
    - 서비스 레이어를 테스트할 때, `Model`에 대한 의존성이 없으면 테스트가 더 간단해집니다. 모델과 관련된 것들이 테스트의 복잡성을 증가시킬 수 있습니다.

### 권장하는 패턴

1. **서비스에서 데이터 처리**:
    
    - 서비스 레이어에서 비즈니스 로직을 처리하고, 필요한 데이터를 반환합니다.
2. **컨트롤러에서 `Model`에 추가**:
    
    - 컨트롤러에서 서비스로부터 데이터를 받아와 `Model`에 추가하여 뷰에 전달합니다.