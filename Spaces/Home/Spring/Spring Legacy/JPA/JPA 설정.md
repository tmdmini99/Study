# 포기 다시 ㄱ 성공


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
		 <!-- https://mvnrepository.com/artifact/commons-dbcp/commons-dbcp -->
        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>


        <!-- -->
			
```
dependencies에 기존에 있던 mybatis는 삭제하고 jpa를 위한 라이브러리 3개를 추가해주었다.

spring-data-jpa는 우리가 사용할 jpa이기 때문에 당연히 필요하고 나머지 둘은 설정을 위해 필요하다.

#### **3. persistence.xml**


```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence 
http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
	<persistence-unit name="jobcall">
		<class>com.poozim.jobcall.model.Work</class>
		
		<properties>
			<!-- <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/> -->
			
			<property name="hibernate.show_sql" value="true"/><!--SQL 쿼리문을 출력한다.-->
			<property name="hibernate.c3p0.min_size" value="5"/>
			<property name="hibernate.c3p0.max_size" value="20"/>
			<property name="hibernate.c3p0.timeout" value="500"/>
			<property name="hibernate.c3p0.idle_test_period" value="2000"/>
		</properties>
	</persistence-unit>
</persistence>
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.1" xmlns="http://xmlns.jcp.org/xml/ns/persistence" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence 
http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
	<!-- 밑에 내용은 내가 생성한 패키지에서 가져오는 것-->
	<persistence-unit name="jobcall">
		<class>com.poozim.jobcall.model.Work</class>
		<class>com.poozim.jobcall.model.WorkBoard</class>
		<class>com.poozim.jobcall.model.WorkBoardFile</class>
		<class>com.poozim.jobcall.model.WorkGroup</class>
		<class>com.poozim.jobcall.model.Member</class>
		<class>com.poozim.jobcall.model.Comment</class>
		<class>com.poozim.jobcall.model.CommentFile</class>
		<class>com.poozim.jobcall.model.FavoritLog</class>
		<class>com.poozim.jobcall.model.StatusLog</class>
		
		<properties>
			<!-- <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/> -->
			
			<property name="hibernate.show_sql" value="true"/><!--SQL 쿼리문을 출력한다.-->
			<property name="hibernate.format_sql" value="true"/><!--쿼리문을 포맷팅하여 보여준다.-->
			<property name="hibernate.use_sql_comments" value="true"/><!--쿼리문 관련 정보를 주석으로 보여준다.-->
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
	
	<!-- <bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<list>
			    <value>classpath:/datasource.properties</value>
			</list>
		</property>
	</bean> -->
	
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
@GeneratedValue(strategy = GenerationType.SEQUENCE)  주로 Oracle, PostgreSQL, DB2와 같은 데이터베이스에서 지원
@GeneratedValue(strategy = GenerationType.IDENTITY)  MySQL


하지만 아직 잘 몰라서 추후에 알아보고 글을 수정하기로 한다.

@Transient는 테이블의 컬럼과 매핑되지 않는, 영속성에서 제외시킬 필드를 지정한다.


@Column
**JPA에서 DB Table의 Column을 Mapping 할 때 `@Column` Annotation을 사용**한다.
DB Table의 Column과 Entity명이 다를 때  사용

|속성|설명|기본값|
|---|---|---|
|name|Mapping할 Column의 이름을 지정.|객체 Field 이름|
|insertable|Entity 저장 시 해당 Field도 저장,  <br>false로 읽기 전용 설정 가능.|true|
|updatable|Entity 수정 시 해당 Field도 수정,  <br>false로 읽기 전용 설정 가능.|true|
|table|하나의 Entity설정에서 두 개이상 Table에 매핑할 때 사용|현재 Class가 매핑된 Table|
|nullable(DDL)|true/false로 null 허용 여부 설정|true|
|unique(DDL)|true/false로 Unique 제약 조건 설정||
|length(DDL)|Column 속성 길이 설정|255|
|columnDefinition(DDL)|DB Column 정보를 직접 설정|Java Type과 설정 DB 방언으로,  <br>적절한 Column Type 생성|
|precision, scale(DDL)|BigDecimal, BigInteger Type에서 사용,  <br>precision은 소수점 포함 전체 자릿수,  <br>scale은 소수 자릿수,  <br>double, float Type에는 적용되지 않음.  <br>아주 큰 숫자나 정밀한 소수를 다룰때 사용.|precision=19, scale=2|

여기서 **`name`, `insertable`, `updatable`, `table`을 제외한 나머지 속성들은 DDL 생성 기능을 사용할 때만 사용되는 속성**들로,

**JPA 실행 로직에는 영향을 끼치지 않는 속성들**이다.

**직접 DDL을 설정하여 DB Table을 구성할 경우 사용할 이유가 없다.**  
_**Entity만으로 개발자가 DB Table 구조 파악이 가능하다는 장점**_

위 속성 중 **`nullable`의 경우 Java의 기본 타입(int, long, ...)은 null 값 입력이 불가능** 하므로,

**`false`를 통해 DB Column에 `Not Null` 제약 조건을 지정해 두는것이 안전**하다.  
_혹은 직접 DB Column에 Not Null 제약 조건 추가_





#### **6. Repository**  여기에 하나 interface WorkRepository extends JpaRepository<Work, Integer>{로 만들었지만 난 안만들었다 굳이 안만들어도 상관 x

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


> Repository <T,ID>  
> CrudRepository <T,ID>  
> PagingAndSotringRepository <T,ID>  
> JpaRepository <T,ID>

```null
T는 Entity의 타입클래스이고 ID는 P.K 값의 Type이다.
```



## **테스트**

컨트롤러에 간단하게 메서드를 하나 넣고 테스트 해보기로 한다.

![[JPA5.png]]

결과

![[JPA6.png]]




Entity.java에 ```java
implements JpaRepository<OpenApiJpaEntity, Long>, QuerydslPredicateExecutor<OpenApiJpaEntity> 추가`



```java
package org.example.entity;  
  
import lombok.Data;  
  
import javax.persistence.Entity;  
import javax.persistence.Id;  
import javax.persistence.Table;  
  
@Data  
@Entity    //  
@Table(name="op")   // 테이블 명과 클래스 명이 다른 경우 사용  Entity == vo
public class OpenApiJpaEntity {  
  
    private String SIGUN_NM; // 시군명  
//    private String SIGUN_CD; // 시군코드  
    private int ACDNT_YY; // 사고년도  
    private String ACDNT_DIV_NM; // 사고유형 구분  
    @Id //id를 무조건 지정  P.K값에 ID 지정정 
    private String MULTI_KNOWLG_DIV_NO; // 다발지식별자  
    private String MULTI_KNOWLG_DIV_GROUP_NO; // 다발지역그룹식별자  
    private String LEGALDONG_CD_NO; // 법정동코드  
    private String SPOT_NO; // 위치코드  
    private String JURISD_POLCSTTN_NM; // 시도시군구명  
    private String LOC_INFO; // 사고지역위치명  
    private int OCCUR_CNT; // 발생건수  
    private int CASLT_CNT; // 사상자수  
    private int DPRS_CNT; // 사망자수  
    private int SERINJRY_INDVDL_CNT; // 중상자수  
    private int SLTINJRY_INDVDL_CNT; // 경상자수  
    private int INJURY_APLCNT_CNT; // 부상자수  
    private double LAT; // 위도  
    private double LOGT; // 경도  
    private String MULTI_REGION_INFO; // 사고다발지역폴리곤정보  
}
```









## 내가 만든 코드

pom.xml -> 원래 쓰던 dependency에 밑에 추가
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
  
<!-- https://mvnrepository.com/artifact/commons-dbcp/commons-dbcp -->  
<dependency>  
    <groupId>commons-dbcp</groupId>  
    <artifactId>commons-dbcp</artifactId>  
    <version>1.4</version>  
</dependency>  
  
  
<!-- -->
```



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
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd       http://www.springframework.org/schema/data/jpa http://www.springframework.org/schema/data/jpa/spring-jpa-1.11.xsd       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">  
  
  
  <!--밑에 username password url driveClassName 설정 -->
    <bean id="jobcallDataSource" class="org.apache.commons.dbcp.BasicDataSource">  
        <property name="username" value="test"/>  
        <property name="password" value="1234"/>  
        <property name="url" value="jdbc:mysql://localhost:3306/test"/>  
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />  
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
  <!-- 내 패키지명으로 변경-->
    <jpa:repositories base-package="org.example.repository" />  
  
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">  
        <property name="entityManagerFactory" ref="entityManagerFactory" />  
    </bean>  
</beans>
```


persistence.xml
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


Entity.java

```java
package org.example.entity;  
  
import lombok.Data;  
  
import javax.persistence.Entity;  
import javax.persistence.Id;  
import javax.persistence.Table;  
  
@Data  
@Entity    //  
@Table(name="op")   // 테이블 명과 클래스 명이 다른 경우 사용  Entity == vo
public class OpenApiJpaEntity {  
  
    private String SIGUN_NM; // 시군명  
//    private String SIGUN_CD; // 시군코드  
    private int ACDNT_YY; // 사고년도  
    private String ACDNT_DIV_NM; // 사고유형 구분  
    @Id //id를 무조건 지정  P.K값에 ID 지정정 
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private String MULTI_KNOWLG_DIV_NO; // 다발지식별자  
    private String MULTI_KNOWLG_DIV_GROUP_NO; // 다발지역그룹식별자  
    private String LEGALDONG_CD_NO; // 법정동코드  
    private String SPOT_NO; // 위치코드  
    private String JURISD_POLCSTTN_NM; // 시도시군구명  
    private String LOC_INFO; // 사고지역위치명  
    private int OCCUR_CNT; // 발생건수  
    private int CASLT_CNT; // 사상자수  
    private int DPRS_CNT; // 사망자수  
    private int SERINJRY_INDVDL_CNT; // 중상자수  
    private int SLTINJRY_INDVDL_CNT; // 경상자수  
    private int INJURY_APLCNT_CNT; // 부상자수  
    private double LAT; // 위도  
    private double LOGT; // 경도  
    private String MULTI_REGION_INFO; // 사고다발지역폴리곤정보  
}
```

Reopository.java

```java
package org.example.repository;  
  
import org.example.entity.OpenApiJpaEntity;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.data.jpa.repository.JpaRepository;  
import org.springframework.stereotype.Repository;  
  
import javax.persistence.EntityManager;  
import javax.persistence.PersistenceContext;  
import java.util.List;  
//Repository == dao
@Repository  
public class OpenApiJpaRepository{  
  
    @Autowired  
    private EntityManager em;  
  
    public List<OpenApiJpaEntity> selectAll(){  
        return em.createQuery("select op from OpenApiJpaEntity op", OpenApiJpaEntity.class).getResultList();  
    }  
  
  
  
  
}
```








---
참조 - https://glow153.tistory.com/25

https://riverblue.tistory.com/47

https://riverblue.tistory.com/49

https://change-words.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%A0%88%EA%B1%B0%EC%8B%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-JPA-%EC%83%9D%EC%84%B1-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-%ED%95%98%EC%9D%B4%EB%B2%84%EB%84%A4%EC%9D%B4%ED%8A%B8-%EC%98%A4%EB%9D%BC%ED%81%B4

https://velog.io/@neity16/4-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8%EC%99%80-JPA-%ED%99%9C%EC%9A%A9-3-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%EA%B5%AC%EC%A1%B0-Service-Repository-%EA%B0%9C%EB%B0%9C


https://velog.io/@modsiw/JPA-Spring-Data-JPA-%EA%B8%B0%EC%B4%88-1