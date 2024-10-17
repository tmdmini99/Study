
Annotation 직접 생성
```java
package com.kpop.merch.common.util;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@Target(ElementType.METHOD) // 메서드에 적용  
@Retention(RetentionPolicy.RUNTIME) // 런타임까지 유지  
public @interface PaginationRequired {  
}
```


Annotation 적용
```java
@PaginationRequired   //직접 적용
@GetMapping("draftOrders")  
public String draftOrdersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {  
  
    List<? extends BasicVo> list = draftOrdersService.selectList(basicParamVo);  
    model.addAttribute("list", list);  
    return "orders/draftOrders";  
}
```

Interceptor에서 사용
```java

@Override  
public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {  
  
    httpServletRequest.setAttribute("ordersCount", commonDao.ordersCount());  
    // PaginationRequired 어노테이션이 붙어 있는 메서드인 경우  
    if (o instanceof HandlerMethod &&  
        ((HandlerMethod) o).hasMethodAnnotation(PaginationRequired.class)) {  
        System.out.println("posthandle");  
        // Pagination 객체를 유틸리티 메서드를 통해 생성  
  
  
        // Request에 Pagination 객체를 저장  
        List<? extends BasicVo> list = (List<? extends BasicVo>) modelAndView.getModel().get("list");  
        Pagination pagination = Pagination.createPagination(httpServletRequest);  
        if (list != null) {  
            System.out.println("Received list size: " + list.size());  
            // 필요한 추가 작업을 수행할 수 있습니다.  
        }  
    }

```

Annotation이 있을경우 If true
```java
if (o instanceof HandlerMethod &&  
        ((HandlerMethod) o).hasMethodAnnotation(PaginationRequired.class)) {  
        System.out.println("posthandle");  
        // Pagination 객체를 유틸리티 메서드를 통해 생성  
  
  
        // Request에 Pagination 객체를 저장  
        List<? extends BasicVo> list = (List<? extends BasicVo>) modelAndView.getModel().get("list");  
        Pagination pagination = Pagination.createPagination(httpServletRequest);  
        if (list != null) {  
            System.out.println("Received list size: " + list.size());  
            // 필요한 추가 작업을 수행할 수 있습니다.  
        }  
    }
```