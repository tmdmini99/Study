
CustomInterceptor.java

```java
package com.kpop.merch.common.interceptor;  
  
import com.kpop.merch.common.dao.CommonDao;  
import com.kpop.merch.common.vo.BasicVo;  
import lombok.RequiredArgsConstructor;  
import lombok.extern.log4j.Log4j2;  
import lombok.extern.slf4j.Slf4j;  
import org.springframework.web.servlet.HandlerInterceptor;  
import org.springframework.web.servlet.ModelAndView;  
  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
  
@Slf4j  
@RequiredArgsConstructor  
public class CustomInterceptor implements HandlerInterceptor {  
  
    private final CommonDao commonDao;  
  
    @Override  
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {  
        return true;  
    }  
  
    @Override  
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {  
        log.info("postHandle");  
        System.out.println("postHandle");  
        httpServletRequest.setAttribute("ordersCount", commonDao.ordersCount());  
    }  
  
    @Override  
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {  
  
    }}
```


servlet-context.xml
```xml
<interceptors>  
    <interceptor>  
        <mapping path="/**"/>  
        <exclude-mapping path="/resources/**"/>  
        <exclude-mapping path="/login"/>  
        <exclude-mapping path="/logout"/>  
        <exclude-mapping path="/error"/>  
        <exclude-mapping path="/common/validator.bms"/>  
        <beans:bean id="commonInterceptor" class="com.kpop.merch.common.interceptor.CustomInterceptor"/>  
    </interceptor>  
</interceptors>
```

여기 부분에 따라 태그가 달라짐 
```xml
xmlns="http://www.springframework.org/schema/mvc
```


별칭을 줄경우 앞에 별칭을 붙여야함
```xml
xmlns:mvc="http://www.springframework.org/schema/mvc"
```


```xml
<mvc:interceptors>  
    <mvc:interceptor>  
        <mvc:mapping path="/**"/>  
        <mvc:exclude-mapping path="/resources/**"/>  
        <mvc:exclude-mapping path="/login"/>  
        <mvc:exclude-mapping path="/logout"/>  
        <mvc:exclude-mapping path="/error"/>  
        <mvc:exclude-mapping path="/common/validator.bms"/>  
        <bean id="commonInterceptor" class="com.gbms.web.common.interceptor.GBMSInterceptor"/>  
    </mvc:interceptor>
</mvc:interceptors>
```


`implements HandlerInterceptor`와 `extends HandlerInterceptorAdapter`의 차이점은 주로 두 가지 방식의 **인터페이스 구현**과 **클래스 상속**에 있습니다. 이 두 가지는 Spring에서 `HandlerInterceptor`를 구현하는 방법을 나타내며, 각각의 사용 방식에 따른 차이점이 있습니다.

### 1. **`implements HandlerInterceptor`** (인터페이스 구현)

`HandlerInterceptor` 인터페이스를 직접 구현하는 방법입니다. `HandlerInterceptor` 인터페이스는 `preHandle`, `postHandle`, `afterCompletion` 세 가지 메서드를 정의하고 있으며, 이를 모두 구현해야 합니다.

#### 특징:

- `HandlerInterceptor` 인터페이스를 구현할 때는 반드시 세 가지 메서드를 오버라이드해야 합니다.
- `HandlerInterceptor` 인터페이스는 기본 구현을 제공하지 않기 때문에, 모든 메서드를 직접 구현해야 합니다.


```java
public class MyHandlerInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // preHandle 로직
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        // postHandle 로직
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // afterCompletion 로직
    }
}
```


### 2. **`extends HandlerInterceptorAdapter`** (클래스 상속)

`HandlerInterceptorAdapter`는 `HandlerInterceptor` 인터페이스를 구현한 추상 클래스입니다. `HandlerInterceptorAdapter`를 상속받으면, `HandlerInterceptor` 인터페이스에서 정의된 세 가지 메서드 중에서 필요한 메서드만 오버라이드하면 되므로, 코드가 더 간결해집니다.

#### 특징:

- `HandlerInterceptorAdapter`는 `HandlerInterceptor`의 기본 구현을 제공하는 추상 클래스입니다.
- `preHandle`, `postHandle`, `afterCompletion` 메서드 중 필요한 메서드만 오버라이드하면 되므로, 불필요한 메서드를 구현할 필요가 없습니다.
- 예를 들어, `preHandle`만 구현하고, 나머지 두 메서드는 그대로 두면 됩니다.


```java
public class MyHandlerInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        // preHandle 로직
        return true;
    }

    // postHandle이나 afterCompletion 메서드는 구현하지 않아도 됩니다.
}
```



### 차이점 요약:

|특징|`implements HandlerInterceptor`|`extends HandlerInterceptorAdapter`|
|---|---|---|
|**구현 방법**|`HandlerInterceptor` 인터페이스를 직접 구현|`HandlerInterceptorAdapter` 추상 클래스를 상속|
|**필수 메서드 구현**|모든 메서드 (`preHandle`, `postHandle`, `afterCompletion`)를 반드시 구현해야 함|필요한 메서드만 오버라이드하면 됨 (나머지 메서드는 기본 구현 사용)|
|**유연성**|모든 메서드를 직접 구현하므로 더 많은 유연성을 제공|필요한 메서드만 구현할 수 있어 코드가 더 간결해짐|
|**코드 길이**|코드가 길어질 수 있음|코드가 간결하고 덜 반복적|

### 결론:

- **`implements HandlerInterceptor`**는 모든 메서드를 직접 구현해야 하므로, 더 많은 제어가 필요할 때 사용합니다.
- **`extends HandlerInterceptorAdapter`**는 불필요한 메서드 구현을 피할 수 있기 때문에, 코드가 더 간결하고 쉽게 작성할 수 있습니다. 주로 기본적인 처리만 필요한 경우에 사용됩니다.

따라서, 특별히 모든 메서드를 오버라이드하여 세밀한 제어가 필요한 경우는 `implements HandlerInterceptor`를 사용하고, 기본적인 처리만 필요한 경우는 `extends HandlerInterceptorAdapter`를 사용하는 것이 일반적입니다.

