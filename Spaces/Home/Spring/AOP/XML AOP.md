Spring XML 기반 트랜잭션 + AOP 설정 설명

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
         http://www.springframework.org/schema/tx
         http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
         http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">
```


## XML 네임스페이스 설명

|네임스페이스|용도|
|---|---|
|`beans`|Spring 기본 Bean 설정|
|`tx`|트랜잭션 설정 (Transaction Management)|
|`aop`|AOP 설정|
|`xsi:schemaLocation`|네임스페이스에 해당하는 XSD 문서 연결|

---

## 🔹 트랜잭션 매니저 설정

```xml
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```


- JDBC 기반의 트랜잭션 관리 설정
    
- `dataSource`는 DB 커넥션을 관리하는 Bean
    

---

## 🔹 어노테이션 기반 트랜잭션 활성화


```xml
<tx:annotation-driven transaction-manager="txManager"/>
```


- `@Transactional` 어노테이션 사용을 가능하게 해줌
    
- 내부적으로 AOP 프록시 생성
    

---

## 🔹 XML 기반 트랜잭션 Advice 설정


```xml
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="insert*" rollback-for="Exception"/>
        <tx:method name="update*" rollback-for="Exception"/>
        <tx:method name="delete*" rollback-for="Exception"/>
        <tx:method name="*Worker" rollback-for="Exception"/>
        <tx:method name="*Worker*" rollback-for="Exception"/>
    </tx:attributes>
</tx:advice>
```


- `txAdvice`: 트랜잭션 적용을 위한 Advice
    
- `rollback-for="Exception"`: 예외 발생 시 rollback
    
- 메서드 이름 패턴으로 트랜잭션 범위를 지정
    

---

## 🔹 AOP 설정 (Pointcut + Advisor)


```xml
<aop:config proxy-target-class="true">
    <aop:pointcut id="requiredTx" expression="
        execution(* com.kwm.web..service.*Service.*Worker(..)) ||
        execution(* com.kwm.web..service.*Service.*Worker*(..)) ||
        execution(* com.kwm.web..service.*Service.insert*(..)) ||
        execution(* com.kwm.web..service.*Service.update*(..)) ||
        execution(* com.kwm.web..service.*Service.delete*(..))"/>
    <aop:advisor id="transactionAdvisor" advice-ref="txAdvice" pointcut-ref="requiredTx"/>
</aop:config>
```



- `proxy-target-class="true"`: CGLIB 기반 프록시 사용 (인터페이스 없어도 가능)
    
- `pointcut`: 트랜잭션이 적용될 메서드 조건 명시
    
- `advisor`: 위 `txAdvice`를 해당 pointcut에 적용
