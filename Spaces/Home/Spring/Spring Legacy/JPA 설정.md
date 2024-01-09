#### **1. Project Facets JPA 추가**

먼저 이전에 만들었던 프로젝트를 JPA 프로젝트로 바꿔야한다.

프로젝트 우클릭 > Properties > Project Facets > JPA체크 후 Apply

처음에는 어떤 경고문구가 있었던거 같은데 기억이 안나지만, 아무 설정 없는채로 일단 만들면 적용이 되었던 것 같다.

Convert가 완료 되면 src/main/java에 META-INF가 생기고 그 폴더 안에 persistence.xml이 생겨있다.

**src/main/java 구조**


![[JPA1.png]]

간단하게 src/main/java 아래 구성을 훑어보면

controller, service, repository, model 패키지로 나눠져있다.

controller와 service는 Mybatis를 사용할때와 동일하다.

repository는 Mybatis의 Mapper라고 생각하면된다.

model은 VO, DTO, domain들을 모아놓는 패키지로 MVC M이다. 개인적으로 이게 편하다.






pom.xml
```xml
<!-- JPA -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-jpa</artifactId>
			<version>2.2.1.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<version>5.1.11.Final</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/javax.persistence/javax.persistence-api -->
		<dependency>
		    <groupId>javax.persistence</groupId>
		    <artifactId>javax.persistence-api</artifactId>
		    <version>2.2</version>
		</dependency>
			
```
dependencies에 기존에 있던 mybatis는 삭제하고 jpa를 위한 라이브러리 3개를 추가해주었다.

spring-data-jpa는 우리가 사용할 jpa이기 때문에 당연히 필요하고 나머지 둘은 설정을 위해 필요하다.

#### **3. persistence.xml**


```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2" xmlns="http://xmlns.jcp.org/xml/ns/persistence" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence 
http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
	<persistence-unit name="jobcall">
		<class>com.poozim.jobcall.model.Work</class>
		
		<properties>
			<!-- <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/> -->
			
			<property name="hibernate.show_sql" value="true"/>
			<property name="hibernate.c3p0.min_size" value="5"/>
			<property name="hibernate.c3p0.max_size" value="20"/>
			<property name="hibernate.c3p0.timeout" value="500"/>
			<property name="hibernate.c3p0.idle_test_period" value="2000"/>
		</properties>
	</persistence-unit>
</persistence>
```

\<class>태그는 JPA의 Entity가 될 위에 언급된 model 패키지 안의 객체 클래스들을 선언하면된다. 나는 테스트를위해 일단 Work클래스에만 Entity를 지정했다.

주석처리된 property가 있다.

```xml
<property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/>
```

hibernate.dialect 경우 사용하는 DB 종류를 지정하는 속성이다. 주석 처리한 이유는 수동으로 지정하지 않아도 자동으로 적용이 되기때문에 굳이 수동으로 했다가 에러 생기면 골치아프다.

```xml
<property name="hibernate.show_sql" value="true"/>
			<property name="hibernate.c3p0.min_size" value="5"/>
			<property name="hibernate.c3p0.max_size" value="20"/>
			<property name="hibernate.c3p0.timeout" value="500"/>
			<property name="hibernate.c3p0.idle_test_period" value="2000"/>
```

이 부분도 마찬가지로 필수 property는 아니다. 하지만 수동으로 지정할 때가 필요할 속성들이라고 생각해서 넣었다.


#### **4. datasource-context.xml**

그부분은 web.xml에 servlet 설정을 보면된다.


![[JPA2.png]]
밑줄친 부분을 보면 appServlet 디렉토리에 context.xml로 끝나는 파일은 모두 적용시키고 있다.



```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:jdbc="http://www.springframework.org/schema/jdbc"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:jpa="http://www.springframework.org/schema/data/jpa"
	xsi:schemaLocation="http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
		http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa-1.11.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
	
	<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
			    <value>classpath:/datasource.properties</value>
			</list>
		</property>
	</bean>
	
	<bean id="jobcallDataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="${JOBCALL_URL}" />
		<property name="username" value="${POOZIM_USER}" />
		<property name="password" value="${POOZIM_PASSWORD}" />
		<property name="maxActive" value="10" />
		<property name="maxIdle" value="5" />
		<property name="maxWait" value="10000" />
		<property name="validationQuery" value="SELECT 1" />
		<property name="testOnBorrow" value="true" />
		<property name="testWhileIdle" value="true" />
		<property name="timeBetweenEvictionRunsMillis" value="7200000" />
		<property name="removeAbandoned" value="true" />
		<property name="removeAbandonedTimeout" value="60" />
		<property name="logAbandoned" value="true" />
	</bean>
	
	<bean id="jpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
	</bean>
	
	<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="jobcallDataSource"></property>
		<property name="jpaVendorAdapter" ref="jpaVendorAdapter"></property>
	</bean>
	
	<jpa:repositories base-package="com.poozim.jobcall.repository" />
	
	<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory" />
	</bean>
</beans>
```

datasource-context.xml 내용이다. 먼저 Namespace에 JPA를 추가해주어야한다. **추가 안하면 \<jpa>를 사용못해서 설정 할 수 없다.**


![[JPA3.png]]


![[JPA4.png]]


```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
			    <value>classpath:/datasource.properties</value>
			</list>
		</property>
	</bean>
```

이부분은 밑에 dbconnect 설정에서 db정보 가리기 위해 properties 파일 사용을 정의한 부분이다. 신경쓰지않아도 된다.

```xml
<bean id="jobcallDataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="${JOBCALL_URL}" />
		<property name="username" value="${POOZIM_USER}" />
		<property name="password" value="${POOZIM_PASSWORD}" />
		<property name="maxActive" value="10" />
		<property name="maxIdle" value="5" />
		<property name="maxWait" value="10000" />
		<property name="validationQuery" value="SELECT 1" />
		<property name="testOnBorrow" value="true" />
		<property name="testWhileIdle" value="true" />
		<property name="timeBetweenEvictionRunsMillis" value="7200000" />
		<property name="removeAbandoned" value="true" />
		<property name="removeAbandonedTimeout" value="60" />
		<property name="logAbandoned" value="true" />
	</bean>
```

dbcp로 디비에 연결하는 datasource 설정이다.

```xml
<bean id="jpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
	</bean>
	
	<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
		<property name="dataSource" ref="jobcallDataSource"></property>
		<property name="jpaVendorAdapter" ref="jpaVendorAdapter"></property>
	</bean>
	
	<jpa:repositories base-package="com.poozim.jobcall.repository" />
	
	<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory" />
	</bean>
```

jpaVendorAdapter와 datasource로 EntityManagerFactory를 설정하고있다. 

그 밑에 jpa:repositories를 설정하고 있는데 repositories의 패키지를 설정해주지 않으면 프로젝트 시작시 Repository에 대한 빈생성을 못하기 때문에 지정을 해야한다. 

근데 내 경우는 이부분에서 계속 org/springframework/core/metrics 관련 오류가 났는데 Spring의 버전과 spring-data-jpa의 버전이 호환이 되지 않아 발생한 에러인듯 하다. 그럴땐 spring의 버전을 올리거나 spring-data-jpa의 버전을 내려주면된다.

#### **5. Entity**

```java
package com.poozim.jobcall.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Transient;

import lombok.Data;

@Data
@Entity	//
@Table(name="Work")	// 테이블 명과 클래스 명이 다른 경우 사용
public class Work {
	@Id	// 고유 값인듯
	@GeneratedValue //auto increament 되는 값
	int seq;
	int member_seq;
	String title;
	String code;
	String email;
	String useyn;
	String regdate;
	String register;
	
	@Transient	//영속 제외 필드
	String search;
}
```

@Data는 롬복 어노테이션으로  getter/setter를 자동으로 넣어주는 어노테이션이다.

@Entity는 디비의 데이터가 매핑될 객체 Entity를 지정한다.

@Table은 테이블과 매핑될 객체의 이름이 다를경우 사용하면된다.

@Id는 테이블의 기본키 값에 매칭되는 필드를 지정하는 어노테이션이다.

@GeneratedValue는 db의 auto_increament, 자동으로 값을 지정하는 어노테이션이다.

사실 좀 더 정확히 말하면

@GeneratedValue(strategy=GenerationType.AUTO)이런 식으로 기본키 생성에 대해 4가지 설정을 할 수 있다.

AUTO 

IDENTITY 

SEQUENCE 

TABLE 

하지만 아직 잘 몰라서 추후에 알아보고 글을 수정하기로 한다.

@Transient는 테이블의 컬럼과 매핑되지 않는, 영속성에서 제외시킬 필드를 지정한다.

#### **6. Repository**

위에 프로젝트 구조에서 봤듯이 일단 WorkRepository 하나만 만들었다.

```java
package com.poozim.jobcall.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.poozim.jobcall.model.Work;

public interface WorkRepository extends JpaRepository<Work, Integer>{
	public List<Work> findAll();
}
```

다른 어노테이션 없이 spring-data-jpa의 JpaRepository를 상속받는 것만으로도 사용이 가능하다.

코드에서는 단순히 selectall 해오는 메서드 하나만 넣어봤다.

**(참고로 커스텀하는게 아니면 메서드 안넣어도 다 사용할 수 있다)**

## **테스트**

컨트롤러에 간단하게 메서드를 하나 넣고 테스트 해보기로 한다.

![[JPA5.png]]

결과

![[JPA6.png]]






---
참조 - https://glow153.tistory.com/25

https://riverblue.tistory.com/47