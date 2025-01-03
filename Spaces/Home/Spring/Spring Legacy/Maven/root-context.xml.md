
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">  
  
    <!-- Root Context: defines shared resources visible to all other web components -->  
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>  
  
    <bean id="jsonView" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView" />  
</beans>
```



멀티파트 요청이 필요할 때
```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>  
<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">  
    <property name="supportedMediaTypes">  
        <list>  
            <value>application/json; charset=UTF-8</value>  
        </list>  
    </property>  
</bean>
```