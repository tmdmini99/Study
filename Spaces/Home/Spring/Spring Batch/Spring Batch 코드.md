

### pom.xml
```xml

<org.springframework.batch-version>3.0.10.RELEASE</org.springframework.batch-version>

<dependency>  
    <groupId>org.springframework.batch</groupId>  
    <artifactId>spring-batch-core</artifactId>  
    <version>${org.springframework.batch-version}</version>  
</dependency>  
<dependency>  
    <groupId>org.springframework.batch</groupId>  
    <artifactId>spring-batch-infrastructure</artifactId>  
    <version>${org.springframework.batch-version}</version>  
</dependency>
```


batch-context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:batch="http://www.springframework.org/schema/batch"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/batch http://www.springframework.org/schema/batch/spring-batch.xsd
            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 배치 잡 빈 정의 -->
    <batch:job id="sampleJob" xmlns="http://www.springframework.org/schema/batch">
        <batch:step id="sampleStep" next="nextStep">
            <batch:tasklet>
                <batch:chunk reader="itemReader" processor="itemProcessor" writer="itemWriter" commit-interval="10"/>
            </batch:tasklet>
        </batch:step>
        <batch:step id="nextStep">
            <!-- 다음 스텝에 대한 설정 -->
        </batch:step>
    </batch:job>

    <!-- ItemReader 빈 정의 -->
    <bean id="itemReader" class="org.springframework.batch.item.file.FlatFileItemReader">
        <!-- ItemReader 설정 -->
    </bean>

    <!-- ItemProcessor 빈 정의 -->
    <bean id="itemProcessor" class="your.item.processor.class">
        <!-- ItemProcessor 설정 -->
    </bean>

    <!-- ItemWriter 빈 정의 -->
    <bean id="itemWriter" class="org.springframework.batch.item.file.FlatFileItemWriter">
        <!-- ItemWriter 설정 -->
    </bean>

    <!-- DataSource 및 트랜잭션 관리자 설정 -->
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
		<!-- 매개변수 --><constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>
	</bean>
	
<!-- 트랜잭션 설정 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>
<!--SimpleAsyncTaskExecutor`는 스프링 프레임워크에서 제공하는 간단한 비동기 작업 실행자 -->
<bean id="taskExecuter" class="org.springframework.core.task.SimpleAsyncTaskExecutor"/>

    <!-- 배치 작업 실행을 위한 JobLauncher 설정 -->
    <bean id="jobLauncher" class="org.springframework.batch.core.launch.support.SimpleJobLauncher">
        <property name="jobRepository" ref="jobRepository" />
        <property name="taskExecutor" ref="taskExecuter"/>
    </bean>

    <!-- JobRepository 설정 -->
    <bean id="jobRepository" class="org.springframework.batch.core.repository.support.JobRepositoryFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="transactionManager" ref="transactionManager" />
        <property name="databaseType" value="your.database.type" />
    </bean>

    <!-- 배치 스텝 실행을 위한 StepScope 설정 -->
    <bean class="org.springframework.batch.core.scope.StepScope">
        <property name="autoProxy" value="true" />
    </bean>

    <!-- 배치 잡 실행을 위한 JobRegistry 설정 -->
    <bean id="jobRegistry" class="org.springframework.batch.core.configuration.support.MapJobRegistry" />

    <bean class="org.springframework.batch.core.configuration.support.JobRegistryBeanPostProcessor">
        <property name="jobRegistry" ref="jobRegistry" />
    </bean>

<!-- mapper  설정 이거 해놓으면 굳이 namespace 쓸 필요 없음 -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.example.mapper" />
</bean>
<!--템플릿 설정 -->
<bean id="jdbcTemplate"  
      class="org.springframework.jdbc.core.JdbcTemplate">  
    <property name="dataSource" ref="dataSource" />  
</bean>


</beans>

```

 >JdbcTemplate이란?
 >
JdbcTemplate은 JDBC 코어 패키지의 중앙 클래스로 JDBC의 사용을 단순화하고 일반적인 오류를 방지하는데 도움이 된다. 개발자가 JDBC를 직접 사용할 때 발생하는 다음과 같은 반복 작업을 대신 처리해준다.
> - 커넥션 획득
> - statement를 준비하고 실행
> - 결과를 반복하도록 루프를 실행
> - 커넥션 종료, statement 및 resultset 종료
> - 트랜잭션을 다루기 위한 커넥션 동기화
> - 예외 발생 시 스프링 예외 변환기 실행


