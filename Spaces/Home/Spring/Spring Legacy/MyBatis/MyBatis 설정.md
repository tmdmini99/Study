



## pom.xml

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->  
<dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis</artifactId>  
    <version>3.5.10</version>  
</dependency>  
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->  
<dependency>  
    <groupId>org.mybatis</groupId>  
    <artifactId>mybatis-spring</artifactId>  
    <version>2.0.7</version>  
</dependency>
```



database-context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" id="propertyPlaceholderConfigurer">
		<property name="location" value="classpath:database/info/dbInfo.properties"></property>
	</bean>
	

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



```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
       xmlns:util="http://www.springframework.org/schema/util"  
       xsi:schemaLocation="http://www.springframework.org/schema/beans  
                           https://www.springframework.org/schema/beans/spring-beans.xsd                           http://www.springframework.org/schema/util                           http://www.springframework.org/schema/util/spring-util-4.0.xsd"        >  
  
    <!-- Root Context: defines shared resources visible to all other web components -->  
    <bean id="dataSourceSpied" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  
        <property name="driverClassName" value="org.postgresql.Driver" />  
        <property name="url" value="jdbc:postgresql://101.101.218.31:5437/kpop_merch" />  
        <property name="username" value="kpop_merch" />  
        <property name="password" value="kenny12!@" />  
        <property name="validationQuery" value="SELECT 1"/> <!-- PostgreSQL -->  
    </bean>  
  
    <bean id="dataSource" class="net.sf.log4jdbc.Log4jdbcProxyDataSource">  
        <constructor-arg ref="dataSourceSpied"/>  
  
        <property name="logFormatter">  
            <bean class="net.sf.log4jdbc.tools.Log4JdbcCustomFormatter">  
                <property name="loggingType"     value="MULTI_LINE"/>  
                <property name="sqlPrefix"       value="&#10;=========================== Query Log ===========================&#10;"/>  
            </bean>  
        </property>  
    </bean>  
  
    <!-- SqlSession -->  
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">  
        <property name="dataSource" ref="dataSource" />  
  
        <!-- MyBatis 설정 파일의 위치를 지정합니다. -->  
        <property name="configLocation" value="classpath:/config/mybatis-config.xml" />  
  
        <!-- SQL 파일의 위치를 지정합니다. -->  
        <property name="mapperLocations" value="classpath*:/sql/**/*.xml" />  
    </bean>  
  
    <!-- SqlSession -->  
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">  
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory" />  
    </bean>  
  
    <!-- 지정된 베이스 패키지에서 DAO(Mapper) 를 검색하여 등록합니다. -->  
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">  
        <property name="basePackage" value="com.kpop.merch" />  
    </bean>  
  
<!--    <util:properties id="fileConfig" location="classpath:/config/file-config.xml"/>-->  
</beans>
```


여기서  이 안에 

```xml
<bean id="dataSourceSpied" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">  
    <property name="driverClassName" value="org.postgresql.Driver" />  
    <property name="url" value="jdbc:postgresql://101.101.218.31:5437/kpop_merch" />  
    <property name="username" value="kpop_merch" />  
    <property name="password" value="kenny12!@" />  
    <property name="validationQuery" value="SELECT 1"/>
</bean>
```

추가하면  세션이 만료되도 끊기지 않는다
```xml
<property name="validationQuery" value="SELECT 1"/>
```
