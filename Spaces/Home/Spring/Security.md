### Spring Security란?

스프링 시큐리티는 인증 (Authentication) ,권한(Authorize) 부여 및 보호 기능을 제공하는 프레임워크다.

Java / Java EE 프레임워크

개발을 하면서 보안 분야는 시간이 많이 소요되는 활동들 중 하나다. Spring Security를 사용함으로써 짜여진 내부 로직을 통해 인증, 권한 확인에 필요한 기능과 옵션들을 제공한다.

Spring Security는 Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크이다. Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하고 있다. 
Filter는 Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 URL 요청을 받지만, Interceptor는 Dispatcher와 Controller사이에 위치한다는 점에서 적용 시기의 차이가 있다. 
Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 개발자 입장에서는 일일이 보안관련 로직을 작성하지 않아도 된다는 장점이 있다.


#### 인증(Authentication) , 인가(Authorization)


![[Security4.png]]

_인증과 인가란 무엇일까? 보통 인증 절차를 거친 후 인가 절차를 진행한다._

- 인증: 해당 사용자가 본인이 맞는지를 확인하는 절차.
- 인가: 인증된 사용자가 요청된 자원에 접근가능한가를 결정하는 절차

**인증 방식**

1. credential 방식: username, password를 이용하는 방식

2. 이중 인증(twofactor 인증): 사용자가 입력한 개인 정보를 인증 후, 다른 인증 체계(예: 물리적인 카드)를 이용하여 두가지의 조합으로 인증하는 방식이다.

3. 하드웨어 인증: 자동차 키와 같은 방식

이중 Spring Security는 credential 기반의 인증을 취합니다.

- principal: 아이디 (username)  보호받는 Resource에 접근하는 대상
- credential: 비밀번호 (password)  Resource에 접근하는 대상의 비밀번호

특정 자원에 대한 접근을 제어하기 위해서는 **권한**을 가지게 된다.

특정 권한을 얻기 위해서는 유저는 인증정보(Authentication)가 필요하고 관리자는 해당 정보를 참고해 권한을 인가(Authorization)한다.

보편적으로 username - password 패턴의 인증방식을 거치기 때문에 **스프링 시큐리티는 principal - credential 패턴**을 가지게 된다.

#### Spring Security의 특징

- Filter를 기반으로 동작한다.
    - Spring MVC와 분리되어 관리하고 동작할 수 있다.
- Bean으로 설정할 수 있다.
    - Spring Security 3.2부터는 XML설정을 하지 않아도 된다.

### 🔊 서블릿과 필터

> 인증과 인가를 담당하는 코드는 기존 서비스와 함께 작성될 수 있다.  
> 하지만, 인증 및 인가 코드는 핵심 비즈니스 로직과는 전혀 관련이 없으며, 보안이 요구되는 모든 컴포넌트에   
>     같은 보안 코드를 반복할 경우, 수정했을 때 아무런 상관없는 영역까지 변경이 전파되는 상당한 큰 단점이 있다.

그렇기에 서블릿 2.3 부터는 필터라는 개념이 추가되었다. 서블릿과 유사하지만, Request와 Response를 먼저 받아  
    조작할 수 있다는 차이점이 존재한다.

스프링 MVC 는 프론트 컨트롤러 패턴을 사용하여, 요청을 처리한다.

쉽게 풀어서 설명하자면, 단 하나의 Dispatcher Servlet 을 이용하여 요청을 받고, 서비스를 추상화한다는 의미이다.

즉, 스프링에서도 필터를 그대로 사용할 수 있다

```java
@ServletComponentScan
@WebServlet(name = "loadFilter", urlPattern = {"/*"}, loadOnStartup = 1}
public class LoadFilter {
    ...
}
```


```java
@Configuration
public class WebConfig {

    @Bean
    public FilterRegistrationBean logFilter() {
        FilterRegistrationBean<Filter> filterBean = new FilterRegistrationBean<>();
        filterBean.setFilter(new LogFilter());
        filterBean.setOrder(1);
        filterBean.addUrlPatterns("/*");
        return filterBean;
    }
}
```

혹은 @ServletComponentScan 과 Filter 인터페이스를 구현한 구현체에 @WebFilter 어노테이션을 추가해주는 방법 역시  사용 가능하다.

### ✂ 스프링 인터셉터

> 서블릿은 자바 웹 표준에서 제공하나, 인터셉터는 스프링 MVC 에서만 제공하는 기능이다.  
> 서블릿과 관계없이 Dispatcher Servlet 에서 컨트롤러에 가기 전에 실행된다.  
> 즉, Spring MVC 의 핵심인 Dispatcher Servlet 에서 동작하는 모든 객체와 예외들에 접근할 수 있다.

- preHandle (boolean)
    - 컨트롤러에 가기전에 호출되는 메소드
    - 반환값이 true 이면 다음 인터셉터가 호출되고, 그렇지 않다면 컨트롤러를 바로 호출한다.
- postHandle (boolean)
    - 컨트롤러의 return 이후 호출되는 메소드
    - 뷰에 전달할 Model 를 조작할 수 있다.
- afterCompletion
    - HTML 뷰가 렌더링된 이후에 호출되는 메소드

#### ⚠ 주의할 점

> postHandle 메소드는 컨트롤러에 예외가 발생할 경우, 호출되지 않는다.  
> 반면에 afterCompletion 메소드는 예외와 상관없이 **항상** 호출된다.

### ⚔ 필터 VS 인터셉터

> 필터는 J2EE 표준 스펙에 정의되어 있는 기술이기에, Spring Framework 에 의존적이지 않다.  
> 반면, 인터셉터는 스프링에서만 사용할 수 있는 기술이다.

![[Security8.png]]

웹 애플케이션에 전역적으로 처리해야하는 기능은 필터로 처리하는 것이 좋고, 클라이언트에 들어오는 디테일한 처리는  인터셉터의 다양한 기능으로 처리하는 것이 좋다고 한다.

## 🌫 스프링 필터

> 스프링에서는 서블릿 요청이 들어오면, 스프링 컨테이너와 연결할 수 있는 DelegationFilterProxy 라는  
>     서블릿 필터를 제공한다.

DelegationFilterProxy 를 통해 스프링 빈으로 구현한 필터를 등록하고, 서블릿 표준 필터처럼 동작하게 된다.

즉, Spring Security 가 Filter 를 사용해서 구현했다는 것은 DelegationFilterProxy 를 통해 필터를 등록했다는 뜻이다.

Spring Security 는 FilterChainProxy 를 통해 다양한 필터 기능을 구현하며, 이는 스프링 빈이기에 DelegationFilterProxy 를

    통해 필터에 추가된다.

### ☄ FilterChainProxy

> Spring Security 에서 제공하는 특수 필터로 다양한 인스턴스를 SecurityFilterChain 을 통해 위임하고 있다.  
> 일종의 스프링 시큐리티를 위한 전용 Dispatcher Servlet 이라 생각하면 된다.


### SecurityFilterChain

![[Security7.png]]

스프링 시큐리티를 이용하면 개발시에 필요한 사용자의 인증, 권한, 보안 처리를 간단하지만 강력하게 구현 할 수 있다. 일반적인 웹 환경에서 브라우저가 서버에게 요청을 보내게 되면, DispatcherServlet(FrontController)가 요청을 받기 이전에 많은 ServletFilter(서블릿 필터)거치게 된다. Security와 관련한 서블릿 필터도 실제로는 연결된 여러 필터들로 구성 되어 있다. 이러한 모습때문에 Chain(체인)이라는 표현을 쓴다.


- SecurityContextPersistenceFilter
    - SecurityContextRepository 에서 SecurityContext 를 조회 및 저장
    - Authentication 객체를 보관하는 시큐리티 컨텍스트를 생성하는 필터
- LogoutFilter
    - 설정된 로그아웃 URL 로 오는 요청을 감시 및 해당 유저 로그아웃 처리
- UsernamePasswordAuthenticationFilter
    - Form 기반 인증에서 설정된 로그인 URL 로 오는 요청을 감시 및 유저 인증 처리
    - AuthenticationManager 를 통한 인증
    - 인증 성공시, 얻은 Authentication 객체를 SecurityContext 에 저장, AuthenticationSuccessHandler 실행
    - 인증 실패시, AuthenticationFailureHandler 실행
- DefaultLoginPageGeneratingFilter
    - 인증을 위한 로그인 폼 URL 감시
- BasicAuthenticationFilter
    - HTTP 기본 인증 헤더를 감시 및 처리
- RequestCacheAwareFilter
    - 로그인 성공 후, 원래 요청 정보를 제구성하기 위해 사용된다.
- SecurityContextHolderAwareRequestFilter
    - HttpServletRequestWrapper 를 상속한 SecurityContextHolderAwareRequestWrapper 클래스로  
            HttpServletRequest 정보를 감싼다.
    - SecurityContextHolderAwareRequestWrapper 는 필터 체인에게 다음 필터들에게 정보를 제공한다.
- AnonymousAuthenticationFilter
    - 사용자 정보가 인증되지 않았다면, 인증 토큰에 사용자가 익명 사용자로 나타난다.
- SessionManagementFilter
    - 인증된 사용자와 관련된 모든 세션을 추적한다.
- ExceptionTranslationFilter
    - 보호된 요청을 처리하는 도중, 발생할 수 있는 예외를 위임 및 전달한다.
- FilterSecurityInterceptor
    - AccessDecisionManager 로써 권한 부여 처리를 위임하여 접근 제어를 쉽게 해준다.
- CorsFilter
	- 허가된 사이트나 클라이언트의 요청인지 검사하는 필터
- CsrfFilter
	- CSRF 공격을 방어하는 필터
- BearerTokenAuthenticationFilter 
	- JWT 인증 필터
- RememberMeAuthenticationFilter 
	- RememberMe 쿠키를 검사하여 인증하는 필터
- HeaderWriterFilter 
	- 응답(Response)에 시큐리티와 관련된 헤더를 설정하는 필터
- WebAsyncManagerIntegrationFilter 
	- 비동기 기능을 사용할때 시큐리티 컨텍스트를 사용할 수 있도록 해주는 필터
### Spring Security Architecture(구조)

![[Security1.png]]

이 사진은 스프링 시큐리티의 아키텍처 사진이고 스프링 시큐리티의 흐름은 아래와 같다.

**1. Http Request 수신** 

-> 사용자가 로그인 정보와 함께 인증 요청을 한다.

**2. 유저 자격을 기반으로 인증토큰 생성** 

-> AuthenticationFilter가 요청을 가로채고, 가로챈 정보를 통해 UsernamePasswordAuthenticationToken의 인증용 객체를 생성한다.

**3. FIlter를 통해 AuthenticationToken을 AuthenticationManager로 위임**

-> AuthenticationManager의 구현체인 ProviderManager에게 생성한 UsernamePasswordToken 객체를 전달한다.

**4. AuthenticationProvider의 목록으로 인증을 시도**

-> AutenticationManger는 등록된 AuthenticationProvider들을 조회하며 인증을 요구한다.

**5. UserDetailsService의 요구**

-> 실제 데이터베이스에서 사용자 인증정보를 가져오는 UserDetailsService에 사용자 정보를 넘겨준다.

**6. UserDetails를 이용해 User객체에 대한 정보 탐색**

-> 넘겨받은 사용자 정보를 통해 데이터베이스에서 찾아낸 사용자 정보인 UserDetails 객체를 만든다.

**7. User 객체의 정보들을 UserDetails가 UserDetailsService(LoginService)로 전달**

-> AuthenticaitonProvider들은 UserDetails를 넘겨받고 사용자 정보를 비교한다.

**8. 인증 객체 or AuthenticationException**

-> 인증이 완료가되면 권한 등의 사용자 정보를 담은 Authentication 객체를 반환한다.

**9. 인증 끝**

-> 다시 최초의 AuthenticationFilter에 Authentication 객체가 반환된다.

**10. SecurityContext에 인증 객체를 설정**

-> Authentication 객체를 Security Context에 저장한다.

최종적으로는 SecurityContextHolder는 세션 영역에 있는 SecurityContext에 Authentication 객체를 저장한다. 사용자 정보를 저장한다는 것은 스프링 시큐리티가 전통적인 세선-쿠키 기반의 인증 방식을 사용한다는 것을 의미한다.

### Spring Security의 주요 모듈


![[Security2.png]]


**1) SecurityContextHolder, SecurityContext, Authentication**

세가지 클래스는 스프링 시큐리티의 주요 컴포넌트로, 각 컴포넌트의 관계를 간단히 표현하자면 다음과 같다.

유저의 아이디와 패스워드 사용자 정보를 넣고 실제 가입된 사용자인지 체크한 후 인증에 성공하면 우리는 사용자의 principal과 credential정보를 Authentication안에 담는다. 스프링 시큐리티에서 방금 담은 Authentication을 SecurityContext에 보관한다. 이 SecurityContext를 SecurityContextHolder에 담아 보관하게 되는 것이다.

![[Security3.png]]

Authentication 클래스는 현재 접근하는 주체의 정보와 권한을 담는 인터페이스고 SecurityContext 저장되며 SecurityContextHolder를 통해 SecurityContext에 접근하고, SecurityContext를 통해 Authentication에 접근할 수 있다.

```java
public interface Authentication extends Principal, Serializable {

	// credentials(주로 비밀번호)을 가져옴
	Collection<? extends GrantedAuthority> getAuthorities();
	
    // credentials(주로 비밀번호)을 가져옴
	Object getCredentials();
	
    
	Object getDetails();
	
	// Principal 객체를 가져옴.
	Object getPrincipal();
	
    // 인증 여부를 가져옴
	boolean isAuthenticated();
	
    // 인증 여부를 설정함
	void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
 
}
```

우리가 로그인한 사용자의 정보를 얻기 위해서는

SecurityContextHolder.getContext().getAuthentication().getPrincipal();

이 구문을 사용하여 가져왔었는데 왜 이렇게 가져올 수 있는지 잘 모르는 분들이 많을 것이다.

하지만 위의 코드를 보면서 이제 이런식으로 동작하는구나 하고 이해할 수 있을 것이다.

**2) UsernamePasswordAuthenticationToken**

이 클래스는 Autentication을 구현한 AbstractAuthenticationToken의 하위의 하위클래스로, 유저의 ID가 Principal의 역할을 하고 유저의 Password가 Credential의 역할을 한다. UserPasswordAuthenticationToken의 첫번째 생성자는 인증 전에 객체를 생성하고, 두번째는 인증이 완료된 객체를 생성한다.

```java
public abstract class AbstractAuthenticationToken implements Authentication, CredentialsContainer {
}
 
public class UsernamePasswordAuthenticationToken extends AbstractAuthenticationToken {
 
	private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
	
	// 주로 사용자의 ID에 해당함
	private final Object principal;
	
	// 주로 사용자의 PW에 해당함
	private Object credentials;
 
	// 인증 완료 전의 객체 생성
	public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
		super(null);
		this.principal = principal;
		this.credentials = credentials;
		setAuthenticated(false);
	}
 
	// 인증 완료 후의 객체 생성
	public UsernamePasswordAuthenticationToken(Object principal, Object credentials,
			Collection<? extends GrantedAuthority> authorities) {
		super(authorities);
		this.principal = principal;
		this.credentials = credentials;
		super.setAuthenticated(true); // must use super, as we override
	}
```

**3) AuthenticationManager**

인증에 대한 부분은 이 클래스를 통해서 처리가 된다. 실질적으로는 AuthenticationManager에 등록된 AuthenticationProvider에 의해서 처리가 된다. 인증에 성공하면 두번째 생성자를 이용해 생성한 객체를 SecurityContext에 저장한다.그리고 인증 상태를 유지하기 위해 세션에 보관하며, 인증이 실패한 경우에는 AuthenticationException를 발생시킨다.

```java
public interface AuthenticationManager {
 
	Authentication authenticate(Authentication authentication) throws AuthenticationException;
 
}


```


```java
public interface AuthenticationManager {
	Authentication authenticate(Authentication authentication) 
		throws AuthenticationException;
}
```

AuthenticationManager를 implements한 ProviderManager는 실제 인증 과정에 대한 로직을 가지고 있는 AuthenticaionProvider를 List로 가지고 있으며, ProividerManager는 for문을 통해 모든 provider를 조회하면서 authenticate 처리를 한다.

```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware,
InitializingBean {
    public List<AuthenticationProvider> getProviders() {
		return providers;
	}
    public Authentication authenticate(Authentication authentication)
			throws AuthenticationException {
		Class<? extends Authentication> toTest = authentication.getClass();
		AuthenticationException lastException = null;
		Authentication result = null;
		boolean debug = logger.isDebugEnabled();
        //for문으로 모든 provider를 순회하여 처리하고 result가 나올 때까지 반복한다.
		for (AuthenticationProvider provider : getProviders()) {
            ....
			try {
				result = provider.authenticate(authentication);

				if (result != null) {
					copyDetails(authentication, result);
					break;
				}
			}
			catch (AccountStatusException e) {
				prepareException(e, authentication);
				// SEC-546: Avoid polling additional providers if auth failure is due to
				// invalid account status
				throw e;
			}
            ....
		}
		throw lastException;
	}
}
```

위에서 설명한 ProviderManager에 우리가 직접 구현한 CustomAuthenticationProvider를 등록하는 방법은 WebSecurityConfigurerAdapter를 상속해 만든 SecurityConfig에서 할 수 있다. WebSecurityConfigurerAdapter의 상위 클래스에서는 AuthenticationManager를 가지고 있기 때문에 우리가 직접 만든 CustomAuthenticationProvider를 등록할 수 있다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
  
    @Bean
    public AuthenticationManager getAuthenticationManager() throws Exception {
        return super.authenticationManagerBean();
    }
      
    @Bean
    public CustomAuthenticationProvider customAuthenticationProvider() throws Exception {
        return new CustomAuthenticationProvider();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.authenticationProvider(customAuthenticationProvider());
    }
}
```



**4) AuthenticationProvider**

이 클래스는 실제 인증에 대한 부분을 처리하는 작업을 치룬다. 인증 전에 Authentication 객체를 받아 인증이 완료된 객체를 반환하는 역할을 하고 아래와 같은 인터페이스를 구현해 Custom한 AuthenticationProvider를 작성하고 AuthenticationManger에 등록하면 된다.

```java
public interface AuthenticationProvider {
	// 인증 전의 Authenticaion 객체를 받아서 인증된 Authentication 객체를 반환
	Authentication authenticate(Authentication authentication) throws AuthenticationException;
 
	boolean supports(Class<?> authentication);
 
}
```

**5) ProviderManager**

AuthenticationManager를 구현한 ProviderManager은 AuthenticationProvider를 구성하는 목록을 갖는다.

```java
public class ProviderManager implements AuthenticationManager, MessageSourceAware, InitializingBean {
	
    public List<AuthenticationProvider> getProviders() {
		return this.providers;
	}
    
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
		Class<? extends Authentication> toTest = authentication.getClass();
		AuthenticationException lastException = null;
		AuthenticationException parentException = null;
		Authentication result = null;
		Authentication parentResult = null;
		int currentPosition = 0;
		int size = this.providers.size();
        
        // for문으로 모든 provider를 순회하여 처리하고 result가 나올때까지 반복한다.
		for (AuthenticationProvider provider : getProviders()) { ... }
	}
}
```

**6) UserDetailsService**

이 클래스는 UserDetails 객체를 반환하는 하나의 메서드만을 가지고 있는데, 일반적으로 이를 구현한 클래스에서 UserRepository를 주입받아 DB와 연결하여 처리한다.

```java
public interface UserDetailsService {
 
	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
 
}
```

**7) UserDetails**

인증에 성공하여 생성된 UserDetails클래스는 Authentication 객체를 구현한 UsernamePasswordAuthenticationToken을 생성하기 위해 사용됩니다. UserDetails를 구현하여 처리할 수 있습니다.
UserDetails 인터페이스의 경우 직접 개발한 UserVO 모델에 UserDetails를 implements하여 이를 처리하거나 UserDetailsVO에 UserDetails를 implements하여 처리할 수 있다.

```java
public interface UserDetails extends Serializable {

    //계정의 권한 목록을 리턴
	Collection<? extends GrantedAuthority> getAuthorities();

	//계정의 비밀번호 리턴
	String getPassword();
	
	//계정의 고유한 값 리턴
	String getUsername();
	
	//계정의 만료 여부 리턴
	boolean isAccountNonExpired();
	
	//계정의 만료 여부 리턴
	boolean isAccountNonLocked();
	
	//isCredentialsNonExpired()
	boolean isCredentialsNonExpired();
	
	//계정의 활성화 여부 리턴
	boolean isEnabled();
 
}
```

**8) SecurityContextHolder**

SecurityContextHolder는 보안 주체의 세부 정보를 포함하여 응용 프로그램의 현재 보안 컨텍스트에 대한 세부 정보가 저장됩니다.
![[Security5.png]]

- SecurityContext 를 현재 스레드와 연결 시켜주는 역할
- 스프링 시큐리티는 같은 스레드 의 어플리케이션 내 어디서든 SecurityContextHolder 의 인증 정보를 확인 가능하도록 구현되어 있는데 이 개념을 `ThreadLocal` 이라고 함.


**9) SecurityContext**

Authentication을 보관하는 역할을 하며, SecurityContext를 통해 Authentication을 저장하거나 꺼내올 수 있습니다.

SecurityContextHolder.getContext().set or get Authentication(authenticationObject);


![[Security6.png]]

- Authentication 의 정보를 가지고 있는 interface
- `SecurityContextHolder.getContext()` 를 통해 얻을 수 있음


**10) GrantedAuthority**

GrantedAuthority는 현재 사용자(Pricipal)가 가지고 있는 권한을 의미하며 ROLE_ADMIN, ROLE_USER와 같이 ROLE_* 형태로 사용합니다. GrantedAuthority객체는 UserDetailsService에 의해 불러올 수 있고, 특정 자원에 대한 권한이 있는지 없는지를 검사해 접근 허용 여부를 결정합니다.

**11) Password Encoding**

AuthenticationManagerBuilder.userDetailsService().passwordEncoder() 를 통해 패스워드 암호화에 사용될 PasswordEncoder 구현체를 지정할 수 있다.

```java
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
	// TODO Auto-generated method stub
	auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());
}

@Bean
public PasswordEncoder passwordEncoder(){
	return new BCryptPasswordEncoder();
}
```


---
참조 - https://velog.io/@hope0206/Spring-Security-%EA%B5%AC%EC%A1%B0-%ED%9D%90%EB%A6%84-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%EC%97%AD%ED%95%A0-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0


https://velog.io/@soyeon207/SpringBoot-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0%EB%9E%80

https://mangkyu.tistory.com/76
https://mangkyu.tistory.com/77

https://velog.io/@eunhye_/Spring-Security


https://workshop-6349.tistory.com/entry/Spring-Security-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0%EB%9E%80

https://huimang2.github.io/java/spring-legacy-security