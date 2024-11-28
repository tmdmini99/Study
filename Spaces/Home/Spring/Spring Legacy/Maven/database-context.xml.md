
```xml
<dependency>  
    <groupId>commons-dbcp</groupId>  
    <artifactId>commons-dbcp</artifactId>  
    <version>1.4</version>  
</dependency>  
  
  
<dependency>  
    <groupId>org.lazyluke</groupId>  
    <artifactId>log4jdbc-remix</artifactId>  
    <version>0.2.7</version>  
</dependency>
```


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!-- db안에 프로퍼티를 주입할때 사용-->
	<!--<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" id="propertyPlaceholderConfigurer">
		<property name="location" value="classpath:database/info/dbInfo.properties"></property>
	</bean>-->
	

	<!-- mybatis 사용하기 위해 객체 생성 -->
	
	<!-- Connection -->
	<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" id="dataSource">
		<property name="username" value="${username}"></property>
		<property name="password" value="${password}"></property>
		<property name="url" value="${url}"></property>
		<property name="driverClassName" value="${driver}"></property>
	</bean>
	
	<bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="configLocation" value="classpath:database/config/MybatisConfig.xml"></property>
		<property name="mapperLocations" value="classpath:database/mappers/*Mapper.xml"></property>
	</bean>
	
	<bean class="org.mybatis.spring.SqlSessionTemplate" id="sqlSession">
		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>
	</bean>

</beans>
```


이거 추가시 dao에서 굳이 namespace로 지정해줄 필요없음 자동으로 매핑
```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.example.mapper" />
</bean>
```


트랜잭션 관리

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
     <property name="dataSource" ref="dataSource"/>
</bean>
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```





connect 재연결

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="${db.driverName}"></property>
    <property name="url" value="${db.url}"></property>
    <property name="username" value="${db.username}"></property>
    <property name="password" value="${db.password}"></property>
    <property name="initialSize" value="${db.initialSize}"></property>
    <property name="maxActive" value="${db.maxActive}"></property>
    <property name="validationQuery" value="SELECT 1"></property> <!-- PostgreSQL -->
</bean>

```


`application.properties` 또는 `application.yml`


```properties
db.driverName=org.postgresql.Driver
db.url=jdbc:postgresql://localhost:5432/your_database
db.username=your_username
db.password=your_password
db.initialSize=5
db.maxActive=10

```




```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  
        <property name="driverClassName" value="${db.driverName}"></property>  
        <property name="url" value="${db.url}"></property>  
        <property name="username" value="${db.username}"></property>  
        <property name="password" value="${db.password}"></property>  
        <property name="initialSize" value="${db.initialSize}"></property>  
        <property name="maxActive" value="${db.maxActive}"></property>         
        **<property name="validationQuery" value="select 1 from dual" /> <—Oracle**

        **<property name="validationQuery" value="select 1" /> <-- MySql**

</bean>  
  
  
  
or  
  
jdbc.properties  
  
#Oracle  
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver  
jdbc.url=jdbc:oracle:thin:@--ip--:1521:ORA11G  
jdbc.username=--id--  
jdbc.password=--pw--  
jdbc.maxActive=100  
jdbc.maxIdle=30  
jdbc.maxWait=-1  
jdbc.validationQuery=select 1 from dual
```



**server.xml 또는 context.xml**


```xml
<Resource name="jdbc/MyDS" auth="Container" type="javax.sql.DataSource"

        username="nextree"

        password="xxx"

        driverClassName="com.mysql.jdbc.Driver"

        url="jdbc:mysql://localhost:3306/nextree"/>
```

**원인**



Tomcat이 DB pool에서 MySQL connection을 물고 있더라도 MySQL 입장에서는 오래된 connection을 문제라고 생각하고 끊어버린다. 오랜만에 애플리케이션에 접속하여 끊어진 connection을 사용할 경우 발생한다.

즉 tomcat이 사용하는 DB pool인 DBCP가 MySQL에 의해 끊어진 connection을 주고 애플리케이션이 이걸 사용하려다가 발생하는 에러이다.

  

**해결책**

  

DBCP가 connection을 돌려주기 전 해당 connection이 살아있는지 검사하는 설정을 해야 한다.


```xml
<Resource name="jdbc/MyDS" auth="Container" type="javax.sql.DataSource"

        username="nextree"

        password="xxx"

        driverClassName="com.mysql.jdbc.Driver"

        url="jdbc:mysql://localhost:3306/nextree"

        **validationQuery="select 1"** />
```


validationQuery 설정은 DBCP가 connection을 반환하기전 설정된 쿼리를 날려 connection이 유효한지 검사하고 유효하지 않다면 다시 연결하여 유효한 connection을 반환한다.

  

이 설정은 대부분의 WAS가 모두 지원하고 운영서버에서 반드시 해주어야하는 설정인데... 신경을 써야한다.

그 외 운영 서버에서  권장되는 설정은 아래 URL을 참조하면 된다.

[http://commons.apache.org/dbcp/configuration.html](http://commons.apache.org/dbcp/configuration.html)




### . **`testWhileIdle`**:

- **값**: `true`
- **설명**:  
    이 설정은 데이터베이스 연결이 **유휴 상태**일 때도 주기적으로 연결이 유효한지 검사할지 여부를 결정합니다.
    - **유휴 연결**이란, 풀에서 "사용되지 않지만" 여전히 존재하는 연결을 말합니다. 예를 들어, 여러 개의 데이터베이스 연결을 풀에 유지하는데, 어떤 연결은 요청이 없어서 일정 시간 동안 사용되지 않을 수 있습니다. 이러한 연결들이 오래된 상태로 풀에 남아 있을 수 있습니다.
    - `testWhileIdle`을 `true`로 설정하면 **유휴 연결도 일정 주기로 검사**해서 여전히 유효한지, 즉 연결이 끊어졌거나 만료되지 않았는지 확인합니다.이렇게 함으로써, 연결이 유효하지 않으면 새로 연결을 만들어 넣고, 유효한 연결은 그대로 두게 되어 시스템의 안정성을 유지할 수 있습니다.

### 2. **`timeBetweenEvictionRunsMillis`**:

- **값**: `30000` (30,000 밀리초 = 30초)
- **설명**:  
    이 설정은 연결 풀에서 유휴 상태의 연결을 **얼마나 자주 검사할지** 결정하는 시간 간격을 밀리초 단위로 설정합니다.
    - 예를 들어, `timeBetweenEvictionRunsMillis`가 `30000`이면, 30초마다 연결 풀에서 유휴 상태인 연결을 검사하게 됩니다.
    - 이 검사 주기는 유휴 연결을 체크하여, 오래되었거나 유효하지 않은 연결을 **정리**하고, 풀에 남아 있는 유효한 연결만 남도록 합니다.너무 짧은 시간 간격을 설정하면 성능에 부담을 줄 수 있고, 너무 긴 시간 간격을 설정하면 오래된 연결이 풀에 남을 수 있으므로 적당한 간격을 설정하는 것이 중요합니다.

### 3. **`minEvictableIdleTimeMillis`**:

- **값**: `60000` (60,000 밀리초 = 60초)
    
- **설명**:  
    이 설정은 연결이 **유휴 상태로 유지되는 최소 시간**을 설정합니다.
    
    - 예를 들어, `minEvictableIdleTimeMillis`가 `60000`이면, 60초 동안 아무 작업 없이 유휴 상태에 있던 연결은 **풀에서 제거**됩니다.
    - 이 시간은 유휴 연결이 너무 오래 유지되지 않도록 하여, 시스템이 필요로 하지 않는 불필요한 연결을 제거하게 만듭니다. 이렇게 하면 시스템 자원을 효율적으로 사용할 수 있습니다.
    
    이 값은 `timeBetweenEvictionRunsMillis`와 연관이 있습니다. `timeBetweenEvictionRunsMillis`는 유휴 연결을 검사하는 주기이고, `minEvictableIdleTimeMillis`는 유휴 상태로 유지될 수 있는 최대 시간을 정의합니다. 두 설정이 함께 동작하여 유휴 상태의 연결을 주기적으로 검사하고, 일정 시간이 지난 연결은 제거할 수 있도록 합니다.
    

### 4. **`testOnBorrow`**:

- **값**: `true`
    
- **설명**:  
    이 설정은 **연결을 빌릴 때마다** 연결이 유효한지를 검사할지 여부를 결정합니다.
    
    - 데이터베이스 연결 풀에서 **연결을 빌릴 때**, 풀에서 가져오는 연결이 여전히 유효한지를 확인하는 것입니다.
    - `testOnBorrow`를 `true`로 설정하면, 연결을 풀에서 가져올 때마다 해당 연결이 "유효한" 상태인지 검사합니다. 이 검사에서 연결이 **끊어졌거나** 유효하지 않다면, 새로운 연결을 만들어 가져옵니다.
    
    이 설정은 특히 연결이 자주 끊어지는 환경에서 유용합니다. 예를 들어, 네트워크 연결이 자주 끊기거나 데이터베이스 서버가 일시적으로 다운되는 경우, 유효하지 않은 연결을 사용하려고 하면 문제가 발생할 수 있기 때문에 이를 미리 방지하는 역할을 합니다.



```xml
<property name="timeBetweenEvictionRunsMillis" value="30000"/> <!-- 유휴 연결 30초마다 검사 -->  
<property name="minEvictableIdleTimeMillis" value="60000"/>   <!-- 유휴 연결 60초 후 제거 -->  
<property name="testOnBorrow" value="true"/>
```



```xml
<bean id="dataSourceSpied" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  
    <property name="driverClassName" value="org.postgresql.Driver" />  
    <property name="url" value="jdbc:postgresql://101.101.218.31:5437/kpop_merch?currentSchema=web" />  
    <property name="username" value="kpop_merch" />  
    <property name="password" value="kenny12!@" />  
    <property name="validationQuery" value="SELECT 1"/>  
    <property name="testWhileIdle" value="true"/>  
    <property name="timeBetweenEvictionRunsMillis" value="30000"/> <!-- 유휴 연결 30초마다 검사 -->  
    <property name="minEvictableIdleTimeMillis" value="60000"/>   <!-- 유휴 연결 60초 후 제거 -->  
    <property name="testOnBorrow" value="true"/>  
</bean>
```