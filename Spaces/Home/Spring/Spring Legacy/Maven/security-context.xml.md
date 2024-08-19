


시큐리티 적용
```xml
<!-- enable use-expressions -->  
    <security:http auto-config="false" use-expressions="true">  
        <security:intercept-url pattern="/resources/**" access="permitAll()"/>  
        <security:intercept-url pattern="/join" access="isAnonymous()"/>  
        <security:intercept-url pattern="/login" access="isAnonymous()"/>  
        <security:intercept-url pattern="/" access="permitAll()"/>  
        <security:intercept-url pattern="/logout" access="permitAll()"/>  
        <security:intercept-url pattern="/common/00599.bms" access="permitAll()"/>  
  
        <!-- iframe 동작처리-->  
        <security:headers >  
            <security:frame-options policy="SAMEORIGIN" />  
        </security:headers>  
<!--        <security:intercept-url pattern="*/*" access="hasAnyRole('ROLE_ADMIN')" />-->  
<!--        <security:intercept-url pattern="/main" access="hasAnyRole('ROLE_ADMIN,ROLE_SIDO,ROLE_USER,ROLE_JOHAP')" />-->  
<!--        <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_ADMIN,ROLE_SIDO,ROLE_USER,ROLE_JOHAP')" />-->  
  
        <security:form-login  
                login-processing-url="/login"  
                authentication-failure-url="/"  
                login-page="/"  
                username-parameter="userId"  
                password-parameter="pwd"  
                authentication-success-handler-ref="gbms0001SuccessHandler"  
                authentication-failure-handler-ref="gbms0001FailHandler"  
        />  
  
        <security:logout logout-url="/logout"  
                         invalidate-session="true"  
                         delete-cookies="JSESSIONID"  
                         success-handler-ref="gbms0001LogoutSuccessHandler"  
        />  
        <!-- enable csrf protection -->  
        <security:csrf />  
  
    </security:http>
```


`<security:csrf />` 태그는 CSRF 보호를 기본적으로 활성화하는 역할을 합니다.
클라이언트 측에서는 서버가 제공하는 CSRF 토큰을 모든 상태 변경 요청(POST, PUT, DELETE 등)에 포함시켜야 하며, 서버 측에서는 이 토큰을 검증하여 CSRF 공격을 방어합니다. 이 설정을 통해 CSRF 공격으로부터 보호할 수 있습니다.