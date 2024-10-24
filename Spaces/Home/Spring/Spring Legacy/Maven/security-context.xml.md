


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



------------------------------------

## 실제로 시큐리티 적용 후 로그인 성공까지 


security-context.xml

```xml
<security:intercept-url pattern="/resources/**" access="permitAll()"/>
```
을 추가해줘야지 로그인 하지 않아도 css적용 가능

```xml
<beans:beans xmlns="http://www.springframework.org/schema/beans"  
             xmlns:security="http://www.springframework.org/schema/security"  
             xmlns:beans="http://www.springframework.org/schema/beans"  
             xmlns:context="http://www.springframework.org/schema/context"  
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
             xsi:schemaLocation="http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd  
                                 http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd                                 http://www.springframework.org/schema/context                                 http://www.springframework.org/schema/context/spring-context.xsd">  
  
    <context:component-scan base-package="com.kpop.merch" />  
  
    <security:http auto-config="true" use-expressions="true">  
        <!-- "/" 경로를 인증된 사용자만 접근 가능 -->  
        <security:intercept-url pattern="/resources/**" access="permitAll()"/>
        <security:intercept-url pattern="/" access="isAuthenticated()" />  
        <security:intercept-url pattern="/login/login" access="permitAll" />  
        <security:intercept-url pattern="/**" access="isAuthenticated()" />  
  
        <!-- 로그인 설정 -->  
        <security:form-login  
        login-processing-url="/login/login"  
        authentication-failure-url="/login/login?error=true"  
        login-page="/login/login"  
        username-parameter="username"  
        password-parameter="password"  
        default-target-url="/"  
        authentication-success-handler-ref="loginSuccessHandler" /> <!-- 성공 핸들러 -->  
  
  
        <!-- 로그아웃 설정 -->  
        <security:logout logout-success-url="/login/login?logout=true" />  
    </security:http>  
  
    <bean id="loginSuccessHandler" class="com.kpop.merch.login.handler.LoginSuccessHandler" />  
    <bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>  
    <bean id="loginAuthenticationProvider" class="com.kpop.merch.login.provider.LoginAuthenticationProvider" />  
    <bean id="loginService" class="com.kpop.merch.login.service.LoginService" />  
    <security:authentication-manager>  
         <security:authentication-provider ref="loginAuthenticationProvider" />  
        <security:authentication-provider>  
            <security:user-service>  
                <security:user name="user" password="{noop}password" authorities="ROLE_USER" />  
            </security:user-service>  
        </security:authentication-provider>  
    </security:authentication-manager>  
  
</beans:beans>
```


form-login을 설정하여 로그인 페이지를 설정
```xml
<security:form-login  
login-processing-url="/login/login"  
authentication-failure-url="/login/login?error=true"  
login-page="/login/login"  
username-parameter="username"  
password-parameter="password"  
default-target-url="/"  
authentication-success-handler-ref="loginSuccessHandler" /> <!-- 성공 핸들러 -->
```

로그인 페이지 설정
```xml
login-page="/login/login"  
```

로그인 버튼 클릭시 이동할 위치
```xml
login-processing-url="/login/login"
```


Encoder 사용하여 bean 생성
```xml
<bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>  
<bean id="loginAuthenticationProvider" class="com.kpop.merch.login.provider.LoginAuthenticationProvider" />  
<bean id="loginService" class="com.kpop.merch.login.service.LoginService" />
```



시큐리티에서 
```xml
<security:authentication-manager>  
     <security:authentication-provider ref="loginAuthenticationProvider" />
 </security:authentication-manager>
```

등록해야지만 provider로 이동


Web.xml 추가 필요

```xml
<filter>  
    <filter-name>springSecurityFilterChain</filter-name>  
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
</filter>  
  
<filter-mapping>  
    <filter-name>springSecurityFilterChain</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```



UserDetail 대신 provider 사용시
```java
package com.kpop.merch.login.provider;  
  
import com.kpop.merch.login.service.LoginService;  
import com.kpop.merch.login.vo.LoginVo;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.security.authentication.AuthenticationProvider;  
import org.springframework.security.authentication.BadCredentialsException;  
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;  
import org.springframework.security.core.Authentication;  
import org.springframework.security.core.AuthenticationException;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.security.crypto.password.PasswordEncoder;  
import org.springframework.stereotype.Component;  
  
@Component  
public class LoginAuthenticationProvider implements AuthenticationProvider {  
  
    @Autowired  
    private LoginService loginService; // DB에서 사용자 정보를 가져오는 서비스  
  
    @Autowired  
    private PasswordEncoder passwordEncoder; // 추가  
  
    @Override  
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {  
        String username = authentication.getName();  
        String password = authentication.getCredentials().toString();  
        LoginVo loginVo = new LoginVo();  
        loginVo.setUsername(username);  
        loginVo.setPassword(password);  
  
        // DB에서 사용자 정보를 조회하여 검증  
        UserDetails user = loginService.login(loginVo);  
        if (user == null || !passwordEncoder.matches(password, user.getPassword())) { // 수정  
            throw new BadCredentialsException("잘못된 자격 증명");  
        }  
  
        return new UsernamePasswordAuthenticationToken(user.getUsername(), null, user.getAuthorities()); // 수정  
    }  
  
    @Override  
    public boolean supports(Class<?> aClass) {  
         return UsernamePasswordAuthenticationToken.class.isAssignableFrom(aClass);  
    }  
}
```



vo에서 implements UserDetails로 UserDetails 상속
```java
  
@Data  
public class LoginVo implements UserDetails {  
    private String username;  
    private String password;  
  
    @Override  
    public Collection<? extends GrantedAuthority> getAuthorities() {  
        return List.of();  
    }  
  
    @Override  
    public boolean isAccountNonExpired() {  
        return false;  
    }  
  
    @Override  
    public boolean isAccountNonLocked() {  
        return false;  
    }  
  
    @Override  
    public boolean isCredentialsNonExpired() {  
        return false;  
    }  
  
    @Override  
    public boolean isEnabled() {  
        return false;  
    }  
}
```


java 8 버전 이하에서 사용시

```java
package com.kpop.merch.login.vo;  
  
import lombok.Data;  
import org.springframework.security.core.GrantedAuthority;  
import org.springframework.security.core.authority.SimpleGrantedAuthority;  
import org.springframework.security.core.userdetails.UserDetails;  
  
import java.util.ArrayList;  
import java.util.Collection;  
import java.util.List;  
  
@Data  
public class LoginVo implements UserDetails {  
    private String username;  
    private String password;  
    private List<GrantedAuthority> authorities;  
  
  
    public void setAuthorities(List<String> authList){  
        List<GrantedAuthority> authorities = new ArrayList<GrantedAuthority>();  
        for(int i=0; i < authList.size(); i++){  
            authorities.add(new SimpleGrantedAuthority(authList.get(i)));  
        }  
        this.authorities = authorities;  
    }  
  
    @Override  
    public Collection<? extends GrantedAuthority> getAuthorities() {  
        return authorities;  
    }  
  
    @Override  
    public boolean isAccountNonExpired() {  
        return false;  
    }  
  
    @Override  
    public boolean isAccountNonLocked() {  
        return false;  
    }  
  
    @Override  
    public boolean isCredentialsNonExpired() {  
        return false;  
    }  
  
    @Override  
    public boolean isEnabled() {  
        return false;  
    }  
}
```




로그인 성공시 handler
```java
package com.kpop.merch.login.handler;  
  
import org.springframework.security.core.Authentication;  
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;  
  
import javax.servlet.ServletException;  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import java.io.IOException;  
  
public class LoginSuccessHandler implements AuthenticationSuccessHandler {  
    @Override  
    public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {  
         // 로그인 성공 후 수행할 작업  
        System.out.println("로그인 성공: " + authentication.getName());  
  
        // 리다이렉트할 URL 설정  
        httpServletResponse.sendRedirect("/");  
    }  
}
```


로그아웃시 csrf로 보내줘야함
```jsp
<form action="/login/logout" method="post">  
                        <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}"/>  
                        <button type="submit" class="btn btn-default btn-flat float-end">Sign out</button>  
                    </form>
```