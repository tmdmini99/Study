
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
public String draftOrdersList(@ModelAttribute("basicParamVo") BasicParamVo basicParamVo, Model model) {  
  
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




```java
import javax.servlet.http.HttpServletRequest;

public class PaginationUtils {
    /**
     * 요청에서 페이지네이션 파라미터를 가져와 BasicParamVo 객체를 업데이트하는 메서드
     *
     * @param request HttpServletRequest 객체
     * @param basicParamVo 페이지 정보가 담길 BasicParamVo 객체
     * @return 업데이트된 BasicParamVo 객체
     */
    public static BasicParamVo createPagination(HttpServletRequest request, BasicParamVo basicParamVo) {
        // 페이지 번호 기본값 설정: 기본값은 1
        int pageNo = basicParamVo.getPageSize() <= 0 ? 1 : basicParamVo.getPageSize(); // pageSize가 0 이하일 경우 1로 설정

        // 페이지 번호를 기본ParamVo에 설정
        basicParamVo.setPageNo(pageNo);

        // 페이지 크기 요청 파라미터 처리
        String pageSizeParam = request.getParameter("pageSize");
        int pageSize = (pageSizeParam != null && !pageSizeParam.isEmpty()) ? Integer.parseInt(pageSizeParam) : 50; // 기본값 50
        basicParamVo.setPageSize(pageSize); // 페이지 크기 설정

        // 업데이트된 BasicParamVo 객체 반환
        return basicParamVo;
    }
}

```



## **JavaDoc**



```java
 /**
     * 요청에서 페이지네이션 파라미터를 가져와 BasicParamVo 객체를 업데이트하는 메서드
     *
     * @param request HttpServletRequest 객체
     * @param basicParamVo 페이지 정보가 담길 BasicParamVo 객체
     * @return 업데이트된 BasicParamVo 객체
     */
```

### 1. **코드의 가독성 향상**

- 다른 개발자나 미래의 자신이 코드를 읽을 때, 메서드가 어떤 기능을 수행하는지 쉽게 이해할 수 있습니다.
- 메서드의 역할, 인자, 반환값에 대한 정보를 제공하므로 코드의 의도를 명확히 전달합니다.

### 2. **자동 문서화**

- JavaDoc 주석을 사용하여 문서화하면, `javadoc` 도구를 이용해 자동으로 API 문서를 생성할 수 있습니다.
- 이 문서는 개발자들에게 메서드에 대한 자세한 설명을 제공하며, 함수 사용법을 쉽게 이해할 수 있게 돕습니다.

### 3. **유지보수 용이**

- 메서드의 기능이나 인자에 대한 설명이 명확하게 되어 있으면, 나중에 코드를 수정하거나 유지보수할 때 도움이 됩니다.
- 코드의 의도를 알 수 있어, 코드 변경 시 실수를 줄이는 데 도움이 됩니다.

### 4. **팀 협업 개선**

- 팀원 간의 협업 시, 메서드의 기능과 사용법이 명확하게 주석으로 설명되어 있으면, 이해하기 쉬워지고 협업이 원활해집니다.
- 특히, 새로운 팀원이 프로젝트에 참여할 때 주석이 많은 도움이 됩니다.