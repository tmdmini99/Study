

```xml
<import resource="classpath:/batch/batch-context.xml"/>
```

java의 import처럼 사용할수 있다.




Beans는 설정 메타 데이터(xml)에 의해 생성이됩니다.


일반적인 bean 선언

```xml
<bean id = "testBean" class = "com.ljw.TestBean" />
```

id : spring container 에서 유일하게 식별할 수 있는 이름

class : 해당하는 bean의 full path (경로)

id대신 name 속성을 사용할 수 있음


```xml
<bean name = "testBean" class = "com.ljw.TestBean" />
```
  
facotry 클래스의 getInstace 를 통한 bean을 설정

```xml
<bean id = "testFactory" class = "com.ljw.TestFactory"
factory-method = "getInstance" />
factory-method : 해당 클래스의 객체를 반환해주는 메소드 (singleton 에서)
```

생성자를 통한 bean 설정

  
```xml
<bean id="testService" class="com.ljw.Service">  
    <constructor-arg>  
        <ref bean="testDao"/>  
    </constructor-arg>  
</bean>  
<bean id="testDao" class="com.ljw.TestDao"/>  

```
      
ref : reference, 즉 testDao id(혹은 name)를 갖는 bean을 생성자의 인자로 넘겨주겠다는 의미      
  
```xml
<bean id="testService" class="com.ljw.Service">  
    <constructor-arg ref="testDao"/>  
</bean>
```

이것 역시 위와 같은 의미  
  
생성자에 특정 값을 넣어줄 때  
```xml
<bean id="testService" class="com.ljw.Service">  
    <constructor-arg>  
        <value> 10  </value>  
     </constructor-arg>  
</bean>  
```

혹은 다음과 같이 작성가능  
```xml
<bean id="testService" class="com.ljw.Service">  
    <constructor-arg value="10"/>  
</bean>  
```
  
```xml
<value type="long"> 3000 </value>
```
 같이 value 값의 type을 지정해줄수도 있다(기본적으로 string으로 인식)  
즉 생성자가 여러가지 유형이 있을경우 위와같이 type을 설정해주지 않으면 기본적으로 string type의 생성자를 먼저 고려하게됨  
  
  
property bean 설정 방식(setXXX 함수 를 이용하여 bean 설정)  
  
```xml
<bean id="testServcie" class="con.ljw.TestService" >  
    <property name="testDao">  
        <ref bean="implTestDao"/>  
    </property>  
</bean>  

<bean id="implTestDao" class="con.ljw.ImplTestDao" />  
```
  
  
property : TestService class의 setTestDao method를 의미  
  
즉 위의 설정은 TestService class의 setTestDao 메소드를 호출하면서 인자 값으로 ImplTestDao 객체를 넘겨준다는 의미  
  
  
```xml
<bean id="urlMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">  
        <property name="mappings">  
            <props>  
                <prop key="/login/login.mw">loginController</prop>  
            </props>  
        </property>  
    </bean>  
```
  
props : java.util.properties 클래스로 key, value를 string type으로만 갖는다.  
즉 위의 예는 SimpleUrlHandlerMapping 클스의  setMappings 메소드를통해 properties 객체를 생성하고  
해당 properties 객체는 key="/login/login.mw", value = loginController를 갖고 있게된다.  
  
  
  
Bean 객체 범위  
```xml
<bean name="testDao" class="com.ljw.TestDao"/>  
```
  
TestDao testDao1 = (TestDao)applicationContext.getBean("testDao");  
TestDao testDao2 = (TestDao)applicationContext.getBean("testDao");  
위 코드를 통해 생성된 testDao는 동일한 객체이다(스프링 컨테이너 내에서 빈 객체는 싱글턴)  
  
bean scope를 명시하여 서로다른 객체로 생성이 가능한데 다음과 같다.  
  
scope="singleon"  : 기본값이며 스프링 컨테이너당 하나의 빈 객체 생성  
scope="prototype" : 빈은 사용할때마다 새로운 객체 생성  
scope="request"    :  http 요청마다 새로운 객체 생성(WebApplicationContext에서 사용)   
scope="session"    :  세션마다 새로운 객체 생성(WebApplicationContext에서 사용)  

 ```xml
  <bean name="testDao" class="com.ljw.TestDao" scope="protytype" />  
```
--> bean의 scope 속성값에 설정 하여 사용  
  
  
list, map 등의 컬렉션 에대한 xml 태크가 있긴하지만 위에 정리된 부분만 봐도  
spring xml에 대해 이해가 될거라 판단됨.




---
출처 -  https://dlgkstjq623.tistory.com/285