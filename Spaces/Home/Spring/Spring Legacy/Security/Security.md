
pom.xml
```xml
        <!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-web -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>5.7.3</version>
        </dependency>
        
        <!-- https://mvnrepository.com/artifact/org.springframework.security/spring-security-config -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>5.7.3</version>
        </dependency>
```


버전은 맞게 설정

/src/main/webapp/WEB-INF/web.xml


```xml
    <!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            /WEB-INF/spring/root-context.xml
            /WEB-INF/spring/security-context.xml
        </param-value>
    </context-param>
    
    <filter>
        <filter-name>springSecurityFilterChain</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>springSecurityFilterChain</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```


`springSecurityFilterChain`이라는 이름으로 `org.springframework.web.filter.DelegatingFilterProxy` 클래스를 필터로 등록하고, 전체 경로에 매핑해줍니다. 이 경우 클라이언트의 모든 요청이 해당 필터를 거치게 됩니다.

`DelegatingFilterProxy`는 서블릿 필터이므로 스프링 컨테이너의 제어를 받지 않습니다. 대신에 스프링에서 지원하는 특수 필터인 `FilterChainProxy`를 감싸고 있어서 해당 필터가 하는 일을 대신하게 됩니다. `FilterChainProxy`는 스프링 빈이므로, 스프링 컨테이너의 제어를 받아서 의존성 빈을 주입할 수 있습니다. 시큐리티에서는 `springSecurityFilterChain`이라는 이름의 객체가 `FilterChainProxy`를 구현하여 스프링 빈으로 등록되어 있으며, `DelegatingFilterProxy`가 해당 빈을 대리자로 등록하기 위해서는 필터이름을 빈의 이름과 동일하게 설정하면 됩니다. 이러한 이유로 `filter-name`을 `springSecurityFilterChain`로 등록합니다.


## 시큐리티 필터

`springSecurityFilterChain`은 많은 필터 리스트로 구성되어 체인을 형성하며, 순서대로 실행됩니다. 일부 필터의 기능은 다음과 같습니다.

|필터|기능|
|---|---|
|WebAsyncManagerIntegrationFilter|비동기 기능을 사용할때 시큐리티 컨텍스트를 사용할 수 있도록 해주는 필터|
|SecurityContextPersistenceFilter|Authentication 객체를 보관하는 시큐리티 컨텍스트를 생성하는 필터|
|HeaderWriterFilter|응답(Response)에 시큐리티와 관련된 헤더를 설정하는 필터|
|CorsFilter|허가된 사이트나 클라이언트의 요청인지 검사하는 필터|
|CsrfFilter|CSRF 공격을 방어하는 필터|
|LogoutFilter|로그아웃 요청을 처리하는 필터|
|UsernamePasswordAuthenticationFilter|username, password를 사용하는 form 기반 인증을 처리하는 필터|
|ConcurrentSessionFilter|동시세션을 제어하는 필터|
|BearerTokenAuthenticationFilter|JWT 인증 필터|
|BasicAuthenticationFilter|Basic 인증 필터|
|RequestCacheAwareFilter|캐시요청을 처리하는 필터|
|SecurityContextHolderAwareRequestFilter|보안 관련 Servlet 3 스펙을 지원하기 위한 필터|
|RememberMeAuthenticationFilter|RememberMe 쿠키를 검사하여 인증하는 필터|
|AnonymousAuthenticationFilter|익명사용자의 요청을 처리하는 필터|
|SessionManagementFilter|세션을 제어하는 필터|
|ExcpetionTranslationFilter|예외 처리 필터|
|FilterSecurityInterceptor|인가를 결정하는 필터|

# 시큐리티 컨텍스트

시큐리티 컨텍스트는 단독으로 설정되어야 하므로 root-context.xml과는 별도의 xml 파일을 작성해야합니다. `security-context.xml` 파일을 새로 만들고 시큐리티 컨텍스트를 설정하겠습니다. 해당 파일을 스프링 컨테이너의 설정파일로 등록해야 하므로 위에서 작성한 `web.xml` 파일에는 `contextConfigLocation`의 파라미터 값으로 `/WEB-INF/spring/security-context.xml`을 추가했습니다.

이제 각종 필터들을 통해 체인을 설정해야하는데 필터의 종류가 많고 복잡하기때문에 기회가 되면 시큐리티에 대해서 글을 따로 작성해보도록 하겠습니다. 이 글에서는 인증에 사용되는 필터체인을 구성해보겠습니다.

## 아키텍처

인증의 아키텍처는 다음과 같습니다.



security-context.xml
```xml
<beans:beans xmlns="http://www.springframework.org/schema/security"  
xmlns:beans="http://www.springframework.org/schema/beans"  
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
xsi:schemaLocation="http://www.springframework.org/schema/beans  
http://www.springframework.org/schema/beans/spring-beans.xsd  
http://www.springframework.org/schema/security  
http://www.springframework.org/schema/security/spring-security.xsd">  

<!-- -->
<context:component-scan base-package="org.exaple"/>

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
만약  Attribute success-handler is not allowed here 이게 뜬다면
`http://www.springframework.org/schema/security/spring-security-5.4.xsd` 여기 버전 확인
ctrl + 마우스 가져다 대면 바로 확인 가능 자신의 시큐리티 라이브러리 버전과 일치시켜야함

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



https://okky.kr/questions/487431 -- token 