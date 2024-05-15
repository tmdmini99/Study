1. `@Component`, `@Service`, `@Repository` 어노테이션 사용: 개별 클래스에 `@Component`, `@Service`, `@Repository` 어노테이션을 직접 추가하여 해당 클래스를 스프링 빈으로 등록할 수 있습니다. 이 방식은 클래스마다 어노테이션을 추가해야 하지만, 클래스에 명시적인 역할을 부여할 수 있습니다.
    
2. `component-scan` 사용: `component-scan`을 설정하여 스프링이 지정한 패키지 내의 클래스들을 스캔하고, `@Component`, `@Service`, `@Repository` 어노테이션이 붙은 클래스들을 자동으로 빈으로 등록할 수 있습니다. 이 방식은 개발자가 클래스마다 어노테이션을 추가할 필요가 없으며, 자동으로 빈으로 등록됩니다.
    

따라서, 두 가지 방법 중 하나를 선택하여 스프링 빈을 등록할 수 있습니다. 어떤 방법을 선택할지는 개발자의 선호도와 프로젝트의 요구 사항에 따라 다를 수 있습니다. 

하지만  같이 쓰는 이유

1. 명시적인 역할 부여: 각 어노테이션은 클래스의 역할을 명시적으로 표시합니다. `@Component`는 일반적인 컴포넌트를 나타내며, `@Service`는 서비스 계층의 컴포넌트, `@Repository`는 데이터 액세스 계층의 컴포넌트를 나타냅니다. 이를 통해 코드의 가독성을 높일 수 있습니다.
    
2. 세분화된 설정: 각 어노테이션은 추가적인 설정을 제공할 수 있습니다. 예를 들어, `@Service` 어노테이션은 트랜잭션 관리를 위한 설정을 추가로 제공합니다. 이를 통해 세분화된 설정이 가능하며, 각 계층에 필요한 기능을 더욱 쉽게 추가할 수 있습니다.
    
3. 확장성과 유연성: `@Component`, `@Service`, `@Repository` 어노테이션을 사용하면, 스프링의 컴포넌트 스캔 기능과 함께 사용할 수 있습니다. 즉, `component-scan`을 설정하여 자동으로 빈으로 등록되는 클래스들 중에서도 각각의 역할을 구분할 수 있습니다. 이를 통해 더욱 확장성과 유연성 있는 애플리케이션 구조를 구성할 수 있습니다.
    
4. 테스트 용이성: `@Component`, `@Service`, `@Repository` 어노테이션을 사용하면, 각 계층을 단위 테스트하기 쉽습니다. 각 계층별로 필요한 의존성을 명시적으로 주입하고, 테스트 환경에서 해당 계층의 기능을 독립적으로 테스트할 수 있습니다.


Annotation 등록할 필요없이 바로 사용 가능


##  component-scan


- **빈으로 등록 될 준비를 마친 클래스들을 스캔하여, 빈으로 등록해주는 것이다.**  
    빈으로 등록 될 준비를 하는 것이 무엇일까?  
    우리가 `@Controller`, `@Service`, `@Component`, `@Repository` 어노테이션을 붙인  
    클래스들이 빈으로 등록 될 준비를 한 것이다.

> component-scan은 기본적으로 @Component 어노테이션을  
> 빈 등록 대상으로 포함한다.  
> 그렇다면 @Controller 나 @Service는 어떻게 인식하는 걸까?  
> 그 이유는 @Controller나 @Service가 @Component를 포함하고 있기 때문이다.


## component-scan 사용방법

component-scan 을 사용하는 방법은  
`xml 파일에 설정하는 방법`, 과 `자바파일안에서 설정하는 방법`이 있다.

**1. xml 파일에 설정**

```xml
<context:component-scan base-package="org.exaple"/>
```

다음과 같이 xml 파일에 설정하고, base package를 적어주면  
base package 기준으로 클래스들을 스캔하여 빈으로 등록한다.  
base package에 여러개의 패키지를 쓸 수 있다.


```xml
<context:component-scan base-package="org.exaple, com.rcod.example"/> 
```


위와 같이 설정하면, base pacakage 하위의 @Controller, @Service  
@Repository, @Component 클래스가 모두 빈으로 등록되므로,  
특정한 객체만 빈으로 등록하여 사용하고 싶다면  
`include-filter`나 `exclude-filter`를 통해 설정할 수 있다.

- exclude-filter

```xml
<context:component-scan base-package="com.rcod.lifelog">
    <context:exclude-filter type="annotation" 
        expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

@Controller 를 제외하고 싶다면 위와 같이 `exclude-filter`를 사용하여  
`org.springframework.stereotype.Controller`를 명시해준다.

- include-filter

```xml
<context:component-scan base-package="com.rcod.lifelog" use-default="false">
    <context:include-filter type="annotation" 
        expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

`use-default="false"`는 기본 어노테이션 @Controller, @Component등을  
스캔하지 않는다는 것이다.  
기본 어노테이션을 스캔하지 않는다고 설정하고, `include-filter`를 통해서  
위와 같이 특정 어노테이션만 스캔할 수 있다.

**2. 자바 파일안에서 설정**

```java
@Configuration
@ComponentScan(basePackages = "com.rcod.lifelog")
public class ApplicationConfig {
}
```

`@Configuration` 은 이 클래스가 xml을 대체하는 설정 파일임을 알려준다.  
해당 클래스를 설정 파일로 설정하고 `@ComponentScan`을 통하여  
`basePackages`를 설정해준다.

- 위와 같이 `component-scan`을 사용하는 두 가지 방법이 있다.  
    만약 component-scan을 사용하지 않으면, 빈으로 설정할 클래스들을  
    우리가 직접 xml 파일에 일일이 등록해 주어야 한다.

```xml
		<bean id="mssqlDAO" class="com.test.spr.MssqlDAO"></bean>
		
		<!-- MemberList 객체에 대한 정보 전달 및 의존성 주입 -->
		<bean id="member" class="com.test.spr.MemberList">
			
			<!-- 속성의 이름을 지정하여 주입 -->
			<property name="dao">
				<ref bean="mssqlDAO"/>
			</property>
		
		</bean>
```

`MssqlDAO` 와 `MemberList`를 빈으로 등록하고,  
MemberList에 Mssql을 주입한 것이다.  
위와 같이 코드가 매우 길어지고, 일일이 추가하기에 복잡해진다.

## component-scan 동작 과정

ConfigurationClassParser 가 Configuration 클래스를 파싱한다.  
@Configuration 어노테이션 클래스를 파싱하는 것이다.  
                   ⬇  
ComponentScan 설정을 파싱한다.  
base-package 에 설정한 패키지를 기준으로  
ComponentScanAnnotationParser가 스캔하기 위한 설정을 파싱한다.  
                   ⬇  
base-package 설정을 바탕으로 모든 클래스를 로딩한다.  
                   ⬇  
ClassLoader가 로딩한 클래스들을 BeanDefinition으로 정의한다.  
생성할 빈의 대한 정의를 하는 것이다.  
                   ⬇  
생성할 빈에 대한 정의를 토대로 빈을 생성한다.


---
참조 - https://velog.io/@hyun-jii/%EC%8A%A4%ED%94%84%EB%A7%81-component-scan-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95  - componetn scan