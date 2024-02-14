
- 컨트롤러 : @Controller (프레젠테이션 레이어, 웹 요청과 응답을 처리함)
- 로직 처리 : @Service (서비스 레이어, 내부에서 자바 로직을 처리함)
- 외부I/O 처리 : @Repository (퍼시스턴스 레이어, DB나 파일같은 외부 I/O 작업을 처리함)


@RequestMapping

우리는 특정 uri로 요청을 보내면 Controller에서 어떠한 방식으로 처리할지 정의를 한다.
이때 들어온 요청을 특정 메서드와 매핑하기 위해 사용하는 것이 @RequestMapping이다.




## **@Controller이란?**

전통적인 Spring MVC의 컨트롤러 어노테이션인 **@Controller는 주로 View(화면)**를 반환하기 위해 사용합니다.

```java
@Controller
public class TestController {
	
    // @RequestMapping(value = "api/board/update", method = {RequestMethod.POST})
    @PostMapping("api/board/update")
    public String update() {
        
        return "update";
    }
}
```

아래와 같은 과정을 통해서 Spring MVC Container는 Client 요청으로부터 View를 반환합니다.
![[Annotation1.png]]

**[과정]**

1. Client는 URL형식으로 요청을 보낸다.
2. DispatcherServlet이 요청을 위임할 Handler Mapping을 찾는다.
3. Handler Mapping을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후 View Name을 Handler Adapter한테 반환한다.
5. Handler Adapter는 이걸 DispatcherServlet한테 반환한다.
6. DispatcherServlet는 View Resolver를 통해 View Name에 해당하는 View를 찾아서 Client한테 반환한다.

위의 과정을 거치면서 View를 찾아서 렌더링 하는 과정을 거치게 됩니다.

하지만 **Spring MVC의 컨트롤러를 사용할 때 Data를 반환**해야 하는 경우도 있습니다.

컨트롤러에서 Data를 반환하기 위해 **@ResponseBody** 어노테이션을 활용해서 JSON 형태의 데이터를 반환할 수 있습니다.


![[Annotation2.png]]

**[과정]**

1. Client는 URL형식으로 요청을 보낸다.
2. DispatcherServlet이 요청을 위임할 Handler Mapping을 찾는다.
3. Handler Mapping을 통해 요청을 Controller로 위임한다.
4. Controller는 요청을 처리한 후 객체를 반환한다.
5. 반환되는 객체는 JSON으로 직렬화(Serialize)돼서 Client에게 반환된다.

컨트롤러를 통해 객체(데이터)를 반환할 대 일반적으로 ResponseEntity로 감싸서 반환합니다.

그리고 객체를 반환하기 위해서는 View를 반환할 때 사용된 View Resolver 대신에 HttpMessageConverter가 동작합니다.

HttpMessageConverter에는 여러 Converter가 등록되어 있고, 반환하는 데이터에 따라 사용되는 Converter가 달라집니다.

단순 문자열인 경우 StringHttpMessageConverter가 사용되고, 객체인 경우에는 MappingJackson2HttpMessageConverter가 사용되고, 데이터의 종류에 따라 서로 다른 MessageConverter가 작동하게 됩니다.

Spring은 Client의 HTTP Accept Header와 서버의 컨트롤러 반환 타입 정보 2개를 조합해서 적합한 HttpMessageConverter를 선택해서 처리합니다. Message Converter가 동작하는 시점은 HandlerAdapter와 Controller가 요청을 주고받는 시점입니다.

**상단 그림**을 보면 4번에서 Message를 객체로 변환, 6번에서는 객체를 Message로 변환하는데 Message Converter가 사용됩니다.




```java
@Controller
@RequiredArgsConstructor
public class TestController {

	private final MemberService memberService;
	
    @GetMapping("api/board/member")
    public @ResponseBody ResponseEntity<Member> findMember(@RequestParam("id") String id) {
        
        return ResponseEntity.ok(memberService.findMember(member));
    }
}
```




## **@RestController 이해하기**

---

### **[ RestController ]**

@RestController는 @Controller에 @ResponseBody가 추가된 것입니다. 당연하게도 RestController의 주용도는 Json 형태로 객체 데이터를 반환하는 것입니다. 최근에 데이터를 응답으로 제공하는 REST API를 개발할 때 주로 사용하며 객체를 ResponseEntity로 감싸서 반환합니다. 이러한 이유로 동작 과정 역시 @Controller에 @ReponseBody를 붙인 것과 완벽히 동일합니다.

- Client는 URI 형식으로 웹 서비스에 요청을 보낸다.
- DispatcherServlet이 요청을 처리할 대상을 찾는다.
- HandlerAdapter을 통해 요청을 Controller로 위임한다.
- Controller는 요청을 처리한 후에 객체를 반환한다.
- 반환되는 객체는 Json으로 Serialize되어 사용자에게 반환된다.




예제 코드
```java
@RestController
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping(value = "/users")
    public User findUser(@RequestParam("userName") String userName){
        return userService.findUser(user);
    }

    @GetMapping(value = "/users")
    public ResponseEntity<User> findUserWithResponseEntity(@RequestParam("userName") String userName){
        return ResponseEntity.ok(userService.findUser(user));
    }
}


```









---
출처 - https://escapefromcoding.tistory.com/732

https://codevang.tistory.com/258

https://gayuna.github.io/spring/spring-bean/


https://backendcode.tistory.com/213

https://mangkyu.tistory.com/49