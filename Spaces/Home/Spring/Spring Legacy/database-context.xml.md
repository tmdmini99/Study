


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