
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