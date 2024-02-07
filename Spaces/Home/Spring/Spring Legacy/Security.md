

security-context.xml
```xml
<beans:beans xmlns="http://www.springframework.org/schema/security"  
xmlns:beans="http://www.springframework.org/schema/beans"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xsi:schemaLocation="http://www.springframework.org/schema/beans  
http://www.springframework.org/schema/beans/spring-beans.xsd  
http://www.springframework.org/schema/security  
http://www.springframework.org/schema/security/spring-security.xsd">  
  
<http>  
    <intercept-url pattern="/member/login" access="permitAll" />  
    <intercept-url pattern="/**" access="isAuthenticated()" />  
<!-- <form-login login-page="/member/login" default-target-url="/home" />-->  
    <form-login  default-target-url="/home" />  
    <logout logout-success-url="/login?logout" />  
</http>  
  
<beans:bean id="authenticationManager" class="org.springframework.security.authentication.ProviderManager">  
    <beans:constructor-arg>  
        <beans:list>  
            <beans:bean class="org.springframework.security.authentication.dao.DaoAuthenticationProvider">  
                <beans:property name="userDetailsService" ref="userDetailsService"/>  
                <beans:property name="passwordEncoder" ref="passwordEncoder"/>  
            </beans:bean>  
        </beans:list>  
    </beans:constructor-arg>  
</beans:bean>  
  
  
<beans:bean id="userDetailsService" class="org.example.service.MemberService">  
    <beans:constructor-arg ref="userDao"/>  
</beans:bean>  
  
<beans:bean id="userDao" class="org.example.dao.MemberDao"/>  
<beans:bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>  
</beans:beans>

```


MemberService
```java
package org.example.service;  
  
import org.example.dao.MemberDao;  
import org.example.dto.MemberDto;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.security.core.userdetails.UserDetailsService;  
import org.springframework.security.core.userdetails.UsernameNotFoundException;  
import org.springframework.stereotype.Service;  
  
@Service  
public class MemberService implements UserDetailsService {  
  
    private  MemberDao memberDao;  
  
    @Autowired  
    public MemberService(MemberDao memberDao) {  
        this.memberDao = memberDao;  
    }  
  
    @Override  
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {  
        MemberDto memberDto = memberDao.getAll();  // 이 메서드가 있어야지 나중에 로그인할수 있음
        return null;  
    }  
}
```



security-context.ml 
만약 
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans:beans xmlns="http://www.springframework.org/schema/security"  
    xmlns:beans="http://www.springframework.org/schema/beans"  
             xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xsi:schemaLocation="http://www.springframework.org/schema/beans  
    http://www.springframework.org/schema/beans/spring-beans.xsd    http://www.springframework.org/schema/security    http://www.springframework.org/schema/security/spring-security-5.4.xsd">  
  
<http auto-config="true">  
    <intercept-url pattern="/member/login" access="permitAll" />  
    <intercept-url pattern="/member/add" access="permitAll" />  
    <intercept-url pattern="/member/test" access="permitAll" />  
    <intercept-url pattern="/**" access="isAuthenticated()" />  
    <form-login login-page="/member/login"  
                username-parameter="id"  
                password-parameter="pw"  
                default-target-url="/"  
                authentication-failure-url="/member/login"  
                authentication-success-handler-ref="SuccesHandler"  
    />  
  
  
<!--    <form-login  default-target-url="/home" />-->  
    <logout logout-success-url="/login?logout" />  
</http>  
  
<beans:bean id="authenticationManager" class="org.springframework.security.authentication.ProviderManager">  
    <beans:constructor-arg>  
        <beans:list>  
            <beans:bean class="org.springframework.security.authentication.dao.DaoAuthenticationProvider">  
                <beans:property name="userDetailsService" ref="userDetailsService"/>  
                <beans:property name="passwordEncoder" ref="passwordEncoder"/>  
            </beans:bean>  
        </beans:list>  
    </beans:constructor-arg>  
</beans:bean>  
  
    <beans:bean id="SuccesHandler" class="org.example.handler.SuccessHandler"/>  
  
    <beans:bean id="userDetailsService" class="org.example.service.MemberService">  
        <beans:constructor-arg index="0" ref="userDao"/>  
        <beans:constructor-arg index="1" ref="passwordEncoder"/>  
    </beans:bean>  
  
<beans:bean id="userDao" class="org.example.dao.MemberDao"/>  
<beans:bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>  
  
<authentication-manager>  
    <authentication-provider user-service-ref="userDetailsService">  
        <password-encoder ref="passwordEncoder"/>  
    </authentication-provider>  
</authentication-manager>  
  
  
</beans:beans>
```




---
참조 - https://huimang2.github.io/java/spring-legacy-security

https://www.robotstory.co.kr/raspberry/?mode=view&board_pid=108

https://velog.io/@jimin3263/Spring-security%EC%9D%98-Filter


https://codecrafting.tistory.com/9?category=958287


https://shxrecord.tistory.com/161?category=662711 - security-context.xml 에러 x 버전이 낮다

https://dev-splin.github.io/spring/Spring-Security/ 이게 될까?



https://github.com/lleellee0/kakaopage-webtoon-downloader/releases/tag/v1.0.0 - 웹툰 다운?


https://velog.io/@leon/posts  jpa

https://github.com/bmm522/quiz-studio/blob/main/spring-module/quizstudio/quiz-batch/src/main/java/com/quizbatch/domain/category/CategoryQueryRepositoryImpl.java  jpa