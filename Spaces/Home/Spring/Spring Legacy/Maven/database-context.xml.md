
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