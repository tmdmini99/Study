


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

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