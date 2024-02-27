AuthenticationProvider 인터페이스는 화면에서 입력한 로그인 정보와 DB에서 가져온 사용자의 정보를 비교해주는 인터페이스이다. 해당 인터페이스에 오버라이드되는 authenticate() 메서드는 화면에서 사용자가 입력한 로그인 정보를 담고 있는 Authentication 객체를 가지고 있다. 그리고 DB에서 사용자의 정보를 가져오는 건 UserDetailsService 인터페이스에서 loadUserByUsername() 메서드로 구현했다. 따라서 authenticate() 메서드에서 loadUserByUsernmae() 메서드를 이용해 DB에서 사용자 정보를 가져와서 Authentication 객체에서 화면에서 가져온 로그인 정보와 비교하면 된다. AuthenticationProvider 인터페이스는 인증에 성공하면 인증된 Authentication 객체를 생성하여 리턴하기 때문에 비밀번호, 계정 활성화, 잠금 모든 부분에서 확인이 되었다면 리턴해주도록 하자.



```java
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.password.PasswordEncoder;

public class CustomAuthenticationProvider implements AuthenticationProvider {
    
    private final UserDetailsService userDetailsService;
    private final PasswordEncoder passwordEncoder;

    public CustomAuthenticationProvider(UserDetailsService userDetailsService, PasswordEncoder passwordEncoder) {
        this.userDetailsService = userDetailsService;
        this.passwordEncoder = passwordEncoder;
    }

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        String username = authentication.getName();
        String password = (String) authentication.getCredentials();
        
        User user = (User) userDetailsService.loadUserByUsername(username);
        
        if (user == null) {
            throw new UsernameNotFoundException("User not found");
        }
        
        if (!passwordEncoder.matches(password, user.getPassword())) {
            throw new BadCredentialsException("Password does not match stored value");
        }
        
        return new UsernamePasswordAuthenticationToken(username, password, user.getAuthorities());
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return UsernamePasswordAuthenticationToken.class.isAssignableFrom(authentication);
    }
}


```


```java
passwordEncoder.matches(password, user.getPassword()
```


`BCryptPasswordEncoder`는 Spring Security에서 제공하는 클래스로, 비밀번호를 안전하게 암호화하는 데 사용됩니다. 이 클래스는 원본 비밀번호를 암호화하고, 저장된 암호화된 비밀번호와 비교하는 기능을 제공합니다.

`matches()` 메서드는 두 개의 파라미터를 받습니다: 사용자가 입력한 원본 비밀번호와 데이터베이스에 저장된 암호화된 비밀번호 입니다. 이 메서드는 입력된 원본 비밀번호를 같은 방법으로 암호화한 후, 결과를 데이터베이스에 저장된 암호화된 비밀번호와 비교합니다. 만약 두 비밀번호가 일치한다면 `true`를, 그렇지 않다면 `false`를 반환합니다.




---
출처 - https://to-dy.tistory.com/87