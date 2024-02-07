
> **ORM(**Object-Relational Mapping)****

**우리가 일반 적으로 알고 있는 애플리케이션 Class와 RDB(Relational DataBase)의 테이블을** **매핑(연결)한다는 뜻이며, 기술적으로는 어플리케이션의 객체를 RDB 테이블에 자동으로 영속화 해주는 것이라고 보면된다.**



![[JPA60.png]]

- ORM(Object-Relatoinal Mapping) - 객체와 DB 데이터 매핑
- JPA - 자바 표준 ORM API, ORM을 사용하기 위한 표준 인터페이스 모음
- 객체를 통해 간접적으로 DB 데이터 다룸
- ORM framwork - JPA 구현체
    - Hibernate
    - EclipseLink


###  **JPA**

Java Persistence Api 이름처럼 **자바 영속성에 관한 api**로 orm에 대한 인터페이스라고 생각하면된다. 사실 JPA는 **표준 명세**를 말하는것이며 RDS데이터를 매핑시킬 객체를 Entity라고 하는데, JPA의 핵심은 이 Enity를 관리하는 **EntityManager**라고 한다. 그러므로 실제로 **JPA를 사용하기위해선 Hibernate와 같이 JPA의 EntityManager를 구현한(Hibernate, DataNucleus, EclipseLink 등) 구현체**를 사용해야한다

![[JPA.png]]


- 현재 자바 진영의 ORM 기술 표준, 인터페이스의 모음이다.  
- 즉 실제로 동작 X  
- JPA 인터페이스를 구현한 대표적인 오픈소스가 Hibernate라고 할 수 있음

- JPA 표준 명세를 구현한 3가지 구현체: Hibernate, EclipseLink, DataNucleus
- 위에 설명한 3가지의 구현체 중 하나를 사용해서 개발을 진행하면 된다.
- 만약 3가지의 구현체가 모두 마음에 들지 않는다면 개발자가 직접 JPA 구현체를 만들어 사용할 수도 있다.

**JPA는 라이브러리가 아닌 ORM을 사용하기 위한 인터페이스의 모음이다.**

이러한 JPA는 인터페이스의 모음, 단순한 명세이기 때문에 구현이 없다. 자바 애플리케이션에서 관계형 데이터베이스를 어떻게 사용할지 정의하는 하나의 방법일 뿐이다.

따라서 이러한 JPA의 구현체 있어야 JPA를 사용할 수 있다.



## Hibernate란?


![[Hibernate1.png]]

**Hibernate는 JPA를 구현한 구현체이다.** 개발된 지 10년이 넘었으며 대중적으로 많이 이용되는 JPA 구현체 중 하나이다.

JPA의 핵심들인 EntityManagerFactory, EntityManager, EntityTransaction 등을 상속받아 구현한다. 

JPA를 구현하는 다른 구현체들로는 EclipseLink나 DataNucleus 등이 있다.

만약 JPA를 구현하는 구현체들이 마음에 들지 않는다면 개발자가 직접 JPA 구현체를 만들어 사용할 수도 있다. 

Hibernate는 내부적으로 JDBC를 이용해 관계형 데이터베이스와 커넥션을 맺고 상호작용한다.


- JPA의 대표 구현체
- 내부에 JDBC 사용
- 간단한 CRUD 쿼리 메서드 제공 -> 메서드로 데이터 조작 가능
- 복잡한 쿼리는 결국 SQL을 짜야함
    - native SQL이 아닌 객체 중심 SQL을 위한 JPQL, querydsl 기술도 있음
- HQL이라 불리는 쿼리 언어 포함
    - HQL은 SQL과 비슷하면서 객체 지향 강점 누릴 수 있음(상속, 다형성 등)
    - 쿼리 결과로 객체를 반환
    - 클래스, 프로퍼티 이름 제외하고 대소문자 구분


## Spring Data JPA란?
**Spring Data JPA는 JPA를 사용하기 편하도록 만들어놓은 모듈이다.** 

Spring Data JPA는 JPA를 한 단계 더 추상화시킨 Repository 인터페이스를 제공한다. 

이러한 Spring Data JPA는 Hibernate와 같은 JPA구현체를 사용해서 JPA를 사용하게 된다. 

Spring Data JPA를 사용하면 사용자는 더욱 간단하게 데이터 접근이 가능해진다. 

Spring Data JPA를 사용하는 방법이나, 더 자세한 내용은 다음에 다뤄보도록 하겠다.



![[JPA59.png]]



#### 장점

- SQL문이 아닌 Method를 통해 DB를 조작할 수 있어, 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있음.  
    (내부적으로는 쿼리를 생성하여 DB를 조작함. 하지만 개발자가 이를 신경 쓰지 않아도됨)
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어, 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높임
- 객체지향적인 코드 작성이 가능하다. 오직 객체지향적 접근만 고려하면 되기때문에 생산성 증가
- 매핑하는 정보가 Class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리
- 예를들어 기존 방식에서 MySQL 데이터베이스를 사용하다가 PostgreSQL로 변환한다고 가정해보면, 새로 쿼리를 짜야하는 경우가 생김. 이런 경우에 ORM을 사용한다면 쿼리를 수정할 필요가 없음

#### 단점

- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있음
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있음
- 학습비용이 비쌈






## Annotation

> **# 어노테이션**

|   |   |
|---|---|
|**어노테이션**|**기능**|
|**@Entity**|JPA를 사용해서 테이블과 매핑할 클레스를 지정한다.|
|**@Table**|엔티티와 맵핑할 테이블을 지정한다. 생략하면 맵핑한  <br>  엔티티 이름을 테이블 이름으로 사용한다|
|**@Column**|엔티티와 맵핑할 테이블의 열을 지정한다|
|**@JoinColumn**|@JoinColumn  <br>  테이블 연관관계를 맵핑할 때 foreign key를 맵핑 할 때  <br>  사용한다.|
|****@JoinColumns****|**@JoinColumn** 을 여러개 사용할 때 사용한다.  <br>  <br>  **@JoinColumns**({  <br>          **@JoinColumn**(),  <br>          **@JoinColumn**()  <br>  })|
|**@OneToOne**|테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 일대일(1:1)의 관계를 맵핑할 때  <br>  설정한다.  <br>  <br>  속성은 ManyToOne을 참조|
|**@OneToMany**|테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 일대다(1:N)의 관계를 맵핑할 때  <br>  설정한다.  <br>  <br>  속성은 ManyToOne을 참조|
|**@ManyToOne** |테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 다대일(N:1)의 관계를 맵핑할 때  <br>  설정한다.|
|*@ManyToMany* |테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 다:다(N:N)의 관계를 맵핑할 때  <br>  설정한다.|
|**@OrderBy -** Hibernate 패키지|해당 열 기준으로 오름차순 내림차순 정렬할 때 사용  <br>  JPA 쿼리문 뒤에 OrderBy 절이 붙게 됨|
|**@Id**|테이블에서 Identity 키로 사용할 필드를 지정|
|**@EmbeddedId** |복합키를 사용할 때 사용 (2개이상의 PK키)|
|**@IdClass**|복합키를 사용할 때 사용 (2개이상의 PK키)|
|**@GeneratedValue**|id 필드의 값을 자동으로 올려주는 역할  <br>  <br>  - 속성 중 strategy = GenerationType.IDENTITY  <br>  사용함으로 DB에 존재하는 실제 id 숫자와의 충돌을 막아줌  <br>  (MySql 과 Oracle 방식이 다름)|
|**@Embedded**|**@Embeddable** 클래스를 타입으로 매핑하는 역할|
|**@Embeddable**|JPA에서는 새로운 값 타입을 직접 정의  <br>  여러 필드가 한 객체로 묶일 수 있을 때 사용  <br>  주로 주소같은 필드에 사용한다.|
|**@Enumerated**|Java의 enum 타입을 컬럼에 맵핑할 때 사용한다.|
|**@Transient**|지정된 필드는 맵핑하지 않는다.  <br>  객체에 임시로 어떤 값을 보관하고 싶을 때 사용한다.  <br>  (Entity 의 List 필드에 주로 사용)|
|**@AttributeOverride**|**@Embeddable** 클래스의 컬럼명, 속성을  <br>  재정의 할 때 사용한다.|
|**@AttributeOverrides**|**@**AttributeOverride****을 여러개 사용할 때 사용한다  <br>       **@AttributeOverrides({  <br>          @AttributeOverride(),  <br>          @AttributeOverride()  <br>   })**|
|**@Temporal**|날짜 타입(java.util.Date, java.util.Calendar)을 맵핑할 때 사용|
|**@CreationTimestamp**|DB 에 INSERT 시 현재 시간을 자동으로 넣어준다.|
|**@UpdateTimestamp**|DB 에 INSERT 시 현재 시간을 자동으로 넣어준다.  <br>  DB 에 UPDATE 시 현재 시간을 자동으로 넣어준다.|
|**@Lob**|데이터베이스 BLOB, CLOB, TEXT 타입과 맵핑한다.  <br>  (대용량 텍스트 - 주로 게시글 내용)|
|**@Query**|JPQL 또는 DB 쿼리문을 직접 작성할 때 사용한다|
|**@Modifying**|@Query Annotation으로 작성 된 변경, 삭제  <br>  쿼리 메서드를 사용할 때 필요  <br>  (INSERT, UPDATE, DELETE = DDL)  <br>  <br>  참고 [https://devhyogeon.tistory.com/4](https://devhyogeon.tistory.com/4)|
|**@ColumnDefault**|INSERT 기본값이 null 아닌 해당값으로 사용할 때 사용  <br>  (해당 열의 기본값)|
|**@Where**|해당 테이블 SELECT(조회) 시 항상 해당 Where 조건으로  <br>  조회 한다. = DB Where 절  <br>  JPA 쿼리문 뒤에 항상 Where 절이 붙게 됨|
|**@DynamicUpdate**|수정된 데이터만 동적으로 Update SQL을 생성한다.|
|**@DynamicInsert**|데이터를 저장할 때 데이터가 존재하는 필드만으로  <br>  Insert  SQL을 동적으로 생성|
|**@SequenceGenerator**|Identity 전략 중 Sequence를 사용하기위해  <br>  Sequence를 설정 및 생성한다.  <br>  (Oracle DB 사용 할 때 @GeneratedValue와 같이 쓰임)|
|**@TableGenerator**|Identity 전략 중 테이블을 사용하기 위해   <br>  시퀀스 테이블을 설정 및 생성한다.|
|**@MappedSuperclass**|Entity 클래스들의 공통적인 부분을 처리할 때 사용한다  <br>  주로 @**CreationTimestamp** 와****@UpdateTimestamp****|
|**@Converter**|컨버터를 사용하면 엔티티의 데이터를 변환해서 데이터베이스에   <br>  저장할 수 있다.  <br>  <br>  예를들어 회원 VIP여부를 자바의 boolean 타입을 사용하고 싶다고  <br>  가정 할 때 JPA를 사용하면 자바의 boolean 타입은 방언에 따라  <br>  다르지만 데이터베이스에 저장될 때 0 또는 1인 숫자로 저장된다.  <br>  그런데 데이터베이스에 숫자 대신 문자 Y 또는 N으로 저장하고 싶다면  <br>  컨버터를 이용하면 된다.|
|**@EntityListeners(AuditingEntityListener.class)**|Entity 를 감시할 때 사용 한다  <br>  @PostLoad(불러올 때), @PrePersist(저장 하기 전),  <br>  @PostPersist(저장 후), @PreUpdate(업데이트 하기 전),  <br>  @PostUpdate(업데이트 후), @PreRemove(삭제 하기 전),  <br>  @PostRemove(삭제 후)|
### 주요 메서드

#### > 1. @Entity

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|엔티티 이름을 지정|클래스명|

**[코드]**
```java
@Entity(name = "user")
public class Member {
 
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
 
    private String userId;
 
    private String password;
 
}
```

**[SQL]**

![[JPA7.png]]

**[기본]**

![[JPA8.png]]

**[name 속성 설명]**


![[JPA9.png]]
DB 테이블명

**DB테이블명**의 **기본값**은

**@Table** 어노테이션의 **name**을 참조하고,

**@Table** 어노테이션 **name**의 기본값은 **@Entity** 어노테이션의 **name**을 참조하고,

**@Entity** 어노테이션 **name**의 기본값은 해당 클래스명을 참조한다.

즉,

**DB테이블명 = @Table(name="@Entity(name="클래스명")")** 

이런 형태가 된다

추가적으로

만약 클래스명이 **Member**가 아니고 **MemberInfo** 였다면

**DB**에 저장될 떄는 **member_info** 로 저장된다.

**대문자**는 **소문자**로 바뀌어서 저장된다.

**@Entity("memberInfo")** 나 **@Table("memberInfo")** 도 똑같이 

**member_info** 로 저장된다

대신 **MEMBERINFO** 이거는 **memberinfo** 로 저장됩니다.

**[참고]**

**Entity 명은 JPQL = @Query를 사용 할 때 사용한다.**

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
 
import java.util.List;
 
public interface MemberRepository extends JpaRepository<Member, Long> {
 
    // @Entity name 기본값 (클래스명) 일 때
    @Query(value = "select member from Member member")
    List<Member> selectAllMember();
 
 
    // @Entity name 을 user 로 재정의 했을 때
    @Query(value = "select entity from user entity")
    List<Member> selectAllUser();
 
}
```


#### > 2. @Table

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|맵핑할 테이블 이름|엔티티 이름을 사용|
|**catalog**|catalog 기능이 있는 DB에서 catalog를 맵핑한다.||
|**schema**|schema 기능이 있는 DB에서 schema를 맵핑한다.||
|**uniqueConstraints**|DDL 생성 시에 유니크 제약조건을 만든다.||

**[코드]**
```java
@Entity(name = "user")

@Table(name = "MEMBER", uniqueConstraints = @UniqueConstraint(columnNames = {"userId","name"}))

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    private String name;

    private String password;

}
```

**[SQL]**

![[JPA10.png]]


**[기본]**


![[JPA11.png]]

상황에 따라 다름

DB에 저장될 때는 소문자로 저장됩니다

클래스명을 MemberEntity 로 사용하는 사람들이 있어서 

@Table(name="member") 를 사용하기도 합니다

**[**uniqueConstraints 속성** 설명]**

**@Table** 어노테이션의 **uniqueConstraints** 속성은 **복합 unique**(2개이상의 unique) 를 할 때 사용되고

**@Column** 어노테이션의 **unique=true** 속성은 **단일 unique**(1개의 unique) 를 할 때 사용한다

**[참고]**
```java
@Entity(name = "user")

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(unique = true)

    private String userId;

    @Column(unique = true)

    private String name;

    private String password;

}
```



![[JPA12.png]]

**@Column(unique=true)**



![[JPA1.gif]]


**@Table uniqueConstraints(...)**


![[JPA2.gif]]

이미지 클릭

|   |   |   |
|---|---|---|
|**입력여부**|**@Column(unique=true)**|**@Table uniqueConstraints(...)**|
|**x1,x1**|**X**|**X**|
|**x1,x2**|**X**|**O**|
|**x2,x1**|**X**|**O**|
|**x2,x2**|**O**|**O**|

#### > 3. @Column


|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|맵핑할 Column 이름|객체의 필드명|
|**nullable**|null 값의 허용 여부를 설정|true|
|**unique**|한 컬럼에 간단히 유니크 제약조건을 걸때 사용한다.||
|**columnDefinition**|DB 컬럼 정보를 직접 줄 수 있다.  <br>  <br>  VARCHAR(255) not null||
|**length**|문자 길이 제약조건. String 타입에만 사용|255|
|**precision, scale**|BigDecimal 타입에서 사용한다.  <br>  precision은 소수점을 포함한 전체 자릿수  <br>  scale은 소수의 자릿수||
|**table**|?|""|
|**updatable**|UPDATE 시 DB에 변경유무   <br>  <br>  false = DB 변경사항 반영안함|true|
|**insertable**|INSERT 시 DB에 저장유무  <br>  <br>  false = DB 저장 안함|true|

**[코드]**

```java
@Entity(name = "user")

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(unique = true, nullable = false, length = 100)

    private String userId;

    @Column(columnDefinition = "varchar(100) not null unique")

    private String name;

    private String password;

}
```

**[SQL]**

![[JPA13.png]]

**[기본]**

![[JPA14.png]]

**[**percision 속성** 설명]**

간혹가다 long 보다 큰 숫자를 써야하는 경우가 있다.

대표적으로는 돈 인데,

워렌버핏과 같은 사람의 돈은 long 타입의 숫자를 넘어선다

이럴 때 BigDecimal 타입을 사용하고 **percision 속성**으로 자릿수를 설정할 수 있다.

_**@Column(**__**precision="") BigDecimal타입**_과 _**Long 타입**_ 예시는 밑에 참고 바람

**[참고]**

**Long**

```java
@Entity(name = "user")

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    private String name;

    private String password;

    private Long money;

}
```




![[JPA15.png]]


![[JPA16.png]]


**BigDecimal**


```java
@Entity(name = "user")

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    private String name;

    private String password;

    @Column(precision = 20)

    private BigDecimal money;

}
```




![[JPA17.png]]



![[JPA18.png]]


#### > 4. @JoinColumn

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|맵핑할 foreign key 이름|필드명 + _ + 컬럼명|
|**referencedColumnName**|foreign key가 참조하는 대상 테이블의 컬럼명|참조하는 테이블의  <br>  기본 키 컬럼명|
|**foreignKey**|foreigh key 제약조건을 직접 지정할 수 있다.  <br>  이 속성은 테이블을 생성할 때만 사용된다.||
|**기타**|@Column에서 사용하는 속성들도 같이 이용할 수 있다. ex) nullable||

**[코드]**

```java
import javax.persistence.Entity;

import javax.persistence.GeneratedValue;

import javax.persistence.GenerationType;

import javax.persistence.Id;

@Entity

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    private String password;

}
```

```java
import javax.persistence.*;

@Entity

public class Board {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @ManyToOne

    @JoinColumn(name = "member_id")

    private Member writer;

    private String title;

    private String content;

}
```


**[SQL]**

![[JPA19.png]]



**[기본]**

![[JPA20.png]]


**[기본 설명]**

**참조키(foreign Key) 일 때 사용한다.**

**@Column** 어노테이션을 포함한다. (즉, 참조키 필드는 **@Column** 대신 **@JoinColumn** 을 사용)

**@JoinColumn** 혼자 쓰면 오류가 발생하기에

**1:1(@OneToOne)**, **1:N(@OneToMany)**, **N:1(@ManyToOne)**, **N:N(@ManyToMany)** 인지 관계를 설정해야한다.

**한 명(One)**의 **유저(Member)**가 **여러개(Many)**의 **게시글(Board)**를 작성할 수 있음으로 

**Board**는 N:1 관계 인 **@ManyToOne 릴레이션(관계) 어노테이션**을 써야한다.

그런데, **릴레이션 어노테이션**은 **@JoinColumn** 없이 혼자 사용할 수 있다 (아래 참고)


![[JPA21.png]]


wirter_id 보다는 member_id 로 정의하여 어디에 참조되어있는지 알기 쉽게하는 것이 좋다

**[referencedColumnName 속성 설명]**

**referencedColumnName** **기본값**은 관계된 테이블의 **PK @Column name** 이기 때문에

**referencedColumnName** **=** "**id**" 가 된다

만약 관계된 테이블에서 **@Column(name="memId")** 로 정의 한다면,

**DB에 컬럼명**은 **mem_id** 이지만,

**referencedColumnName** **="memId"** 로 사용해야한다.

**name**을 재정의 한다면, **referencedColumnName** 도 같이 써주어야 한다

**자세한 이유는 #5. @JoinColumns 링크 필수로 참고 바람**

**[**foreignKey** 속성 설명]**

**foreignKey** 를 지정하지 않을 경우 기본값으로

![[JPA22.png]]

처럼 알 수 없는 문자로 이름이 지어진다

이름을 재정의하기 위해서 **foreignKey** 를 사용한다


![[JPA23.png]]


**[기타 속성 설명]**

**@Column** 의 속성들을 사용할 수 있다.

**nullable**

**updatable**

**insertable**

**unique**

**columnDefinition**

#### > 5. @JoinColumns


![[JPA24.png]]

**[설명]**

**@JoinColumn** 을 여러개 사용할 때 사용한다

**referencedColumnName** 은 관계된 테이블의 **id 필드명**이다

**private Long first_id;**

**[필수 참고]**

 [JPA JoinColumns 사용시 주의 사항](https://medium.com/@SlackBeck/jpa-joincolumns-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%A3%BC%EC%9D%98-%EC%82%AC%ED%95%AD-7bc22b98ed9b)

 [공유된 FK(Foreign Key) JPA 연관 관계 매핑 하기](https://medium.com/@SlackBeck/%EC%A4%91%EC%B2%A9%EB%90%9C-fk-foreign-key-%EB%A5%BC-jpa%EB%A1%9C-%EC%97%B0%EA%B4%80-%EA%B4%80%EA%B3%84-%EB%A7%A4%ED%95%91-%ED%95%98%EA%B8%B0-216ba5f2b8ed)

#### > **#6. @OneToOne**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**optional**|**false로 설정하면 연관된 엔티티가 항상 있어야 한다.**|true|
|true 는 left outer join 을 사용한다|
|false 는 inner join 을 사용한다|
|**orphanRemoval**|**true**|false|
|**false**||
|true 인 경우  <br>  해당 필드를 UPDATE 쿼리문으로 null 로 설정할 경우  <br>  관계된 테이블 레코드들은 모두 삭제된다.||
|DELETE 쿼리문과 다른 점은  <br>  DELETE는 해당 레코드를 삭제하면 관계된 테이블의  <br>  레코드들을 모두 삭제하는 반면,  <br>  orphanRemoval 은 해당 테이블의 해당 열이 Null 값 으로   <br>  UPDATE 한 경우, 관계된 테이블의 레코드들을 모두 삭제  <br>  한다.||
|**fetch**|**FetchType.EAGER**   <br>  <br>  관계된 테이블의 정보를 미리 읽어온다  <br>  <br>  join 을 이용한 1개의 SELECT 쿼리문만 실행된다  <br>  <br>  optional 로 out join과 inner join 설정 가능 (기본 out join)|FetchType.EAGER|
|**FetchType.LAZY**  <br>    <br>  실제로 요청하는 순간에만 읽어온다.  <br>  <br>  2개의 SELECT 쿼리문이 실행된다|
|**cascade**|**영속성 전이 기능을 사용한다.**||
|**CascadeType.ALL**  <br>  <br>  CascadeType 5개 전부 사용|
|**CascadeType.PERSIST**  <br>  <br>  INSERT 시 연결된 테이블도 save()|
|**CascadeType.MERGE**  <br>  <br>  UPDATE 시 연결된 테이블도 save()|
|**CascadeType.REMOVE**  <br>  <br>  DELETE 시 연결된 테이블도 delete()|
|**CascadeType.DETACH**|
|**CascadeType.REFRESH**|
|**targetEntity**|**관계를 맺을 Entity Class를 정의한다.**|void.class|
|**mappedBy**|**양방향 관계 설정시 관계의 주체가 되는 쪽에서 정의합니다.**|""|
|**mappedBy="[관계된 테이블의 필드명]"**|
|**mappedBy 를 쓰는 경우는 관계의 주인이 아니기 때문에  <br>  SELECT 역할만 한다.**|
|****주인 테이블은 @ManyToOne 과 @JoinColumn 사용하며  <br>  SELECT, INSERT, UPDATE, DELETE 역할을 한다.****|
|[https://gmlwjd9405.github.io/2019/08/14/bidirectional-association.html](https://gmlwjd9405.github.io/2019/08/14/bidirectional-association.html)|

**[단방향 & 양방향 간단 정리]**


![[JPA25.png]]


**[optional **속성** 설명]**

**optional = false** 일 경우에는 관계된 테이블의 데이터가 반드시 있어야 합니다.

**@Column** 의 **nullable=false** + **inner join**

```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @Autowired

    private BlockRepository blockRepository;

    @PostMapping("/member")

    public void boardCreate(@RequestBody Member member) {

        memberRepository.save(member);

    }

    @PostMapping("/block/{userId}")

    public void blockCreate(@PathVariable("userId") Long userId) {

        Member member = memberRepository.findById(userId).get();

        Block block = new Block();

        block.setName(member.getUserId());

        block.setMember(member);

        blockRepository.save(block);

    }

}
```


```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(name = "user_id")

    private String userId;

    private String password;

}
```

```java
@Entity

@Setter

@Getter

public class Block {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String name;

    @OneToOne(fetch = FetchType.EAGER, optional = true)

    private Member member;

}
```




![[JPA3.gif]]



![[JPA26.png]]


![[JPA27.png]]



![[JPA4.gif]]



![[JPA28.png]]


![[JPA29.png]]


[fetch 속성 설명]

**Api.java (@RestController)**
```java
@GetMapping("/boards/{boardId}")

public Board boardDetail(@PathVariable("boardId") Long boardId) {

    Board board = boardRepository.findById(boardId).get();

    log.info("board Title ->{}", board.getTitle());

    log.info("board Id ->{}", board.getId());

    log.info("board Writer ->{}", board.getWriter());

    return board;

}
```

**- FetchType.EAGER (즉시로딩)**



![[JPA30.png]]

****- FetchType.LAZY(지연로딩)****

![[JPA31.png]]

**차이점**은

**EAGER (즉시로딩)**은 Join 을 이용하여 **1개의 SQL문**으로 처리하였고,

**LAZY (지연로딩)**은 **2개의 SQL문**으로 처리하였다.

네트워크 연결이 필요한 작업은 최소하 하는 것이 가장 바람직한 코드이다.

DB에서 1번에 가져올 것을 100번이나 요청에서 가져오는 방법은 효율적이지 못한 방법이다.

이렇게 따지고보면 **EAGER**는 속도에 효율적인 방법이라고 봐야하는게 맞는가?

아니다, **EAGER**를 써야할 때와 **LAZY** 를 써야할 때가 있기 때문에 상황마다 다르다

보통은 **LAZY** 사용하고 **EAGER**를 사용하는 경우는 드믈다.

**[LAZY를 사용하는 이유]**

1. 연관된 테이블의 정보가 필요없는 경우

![[JPA32.png]]

설명을 위해 양방향 매핑으로 예시를 들었습니다

**EAGER**를 사용하여 릴레이션(관계) 된 카테고리 테이블의 정보까지 굳이 조회할 필요는 없다.

게시글 상세내용보기를 눌렀을 때 주는 것이 바람직하다.

SQL 문이 1번만 실행되었다고 하였지만 오히려 필요없는 많은 데이터양을 조회하는 것에 효율적이지 못하다.

하지만 **LAZY** 방식의 코드 그림을 보면

**board.getTitle(); board.getId();** GET 메소드를 사용할 때가 아닌

**board.getWriter();** GET 메소드를 사용할 때

**DB에서 조회 후** **GET 메소드가 실행**되는 것을 볼 수 있다.

즉, 필요할 때 만 사용한다.

(**toString** 도 똑같이으로 해당 필드 호출 시에 DB 조회 합니다)

**2. 연관된 테이블이 서버에서 작업처리 도중 변경된 경우**

![[JPA33.png]]

게시판은 큰 문제 될 것 없지만, 결제나, 주문 같은 경우 문제가 된다

B 사용자가 사용하는 Board 객체에는 Member 객체까지 세팅이 되어있다.

그런데 해당 객체를 이용하여 서버에서 작업하는 도중에

DB Member 테이블의 A 사용자의 이름이 root 에서 benny 로 바뀌었다고 해보자

그러면 B 사용자에 출력되는 게시판에 A 사용자가 작성한 게시글의 이름은

benny 가 아닌 root 가 된다.

물론 다시 새로고침하면 문제가 될 것은 없지만,

만약 주문이나 결제와 같은 경우 주문취소와 관련된 경우는 큰 문제가 될 수 있다.

---

**[**orphanRemoval** 속성 설명]**

**orphanRemoval** 은 고아객체를 의미하면

**false**는 부모가 없는 고아객체 존재할 수 있게하고

**true**는 고아객체가 존재할 수 없게한다.

즉, 고아객체란

관계된 테이블에서 부모테이블에서는 값이 사라져있는데 자식테이블에는 값이 존재하는 경우이다.

```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @PostMapping("/memberBlock/{userId}")

    public void boardCreate(@PathVariable("userId") Long id) {

        log.info("userId -> {}", id);

        Member member =  memberRepository.findById(id).get();

        Block block = new Block(member.getUserId());

        member.setBlock(block);

        memberRepository.save(member);

    }

    @PostMapping("/orphanRemoval/{userId}")

    public void orphanRemoval(@PathVariable("userId") Long userId) {

        log.info("userId -> {}", userId);

        Member member =  memberRepository.findById(userId).get();

        member.setBlock(null); // orphanRemoval

        memberRepository.save(member);

    }

}
```


```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(name = "user_id")

    private String userId;

    private String password;

    @OneToOne(cascade = {CascadeType.PERSIST, CascadeType.MERGE}, orphanRemoval = true)

    private Block block;

}
```


**cascade 는 저장될 때 관계된 Block 에 INSERT 와 UPDATE 작업이 되기 위해 작성하였습니다**

```java
@Entity

@Setter

@Getter

@NoArgsConstructor

public class Block {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String name;

    public Block(String name) {

        this.name = name;

    }

}
```


![[JPA5.gif]]

orphanRemoval=true [이미지 클릭]

![[JPA6.gif]]

orphanRemoval=false [이미지 클릭]

**member.setBlock(null); 이 부분에서 Member와 Block 간의 관계가 끊기는 것인데**

**true 인 경우 관계가 끊어진 경우는 삭제하고,**

**false 인 경우는 관계만 끊어지고 Block 의 데이터는 남아있는 모습을 볼 수 있습니다**

---

**[cascade 속성]**

**CascadeType.PERSIST**

[**INSERT**] **save()** 시 관계된 테이블도 [**INSERT**] **save()** 됩니다.

**CascadeType.MERGE**

[**UPDATE**] **save()** 시 관계된 테이블도 [**UPDATE**] **save()** 됩니다.

**CascadeType.REMOVE**

[**DELETE**] **delete()** 시 관계된 테이블도 [**DELETE**] **delete()** 됩니다.

---

**[mappedBy 속성]**
```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @Autowired

    private BlockRepository blockRepository;

    @PostMapping("/member")

    public void boardCreate(@RequestBody Member member) {

        memberRepository.save(member);

    }

    @PostMapping("/block/{userId}")

    public void blockCreate(@PathVariable("userId") Long userId) {

        Member member = memberRepository.findById(userId).get();

        Block block = new Block();

        block.setName(member.getUserId());

        block.setMember(member);

        blockRepository.save(block);

    }

    @GetMapping("/block/{blockId}")

    public Block blockDetail(@PathVariable("blockId") Long blockId) {

        return blockRepository.findById(blockId).get();

    }

    @GetMapping("/members/{memberId}")

    public Member memberDetail(@PathVariable("memberId") Long memberId) {

        return memberRepository.findById(memberId).get();

    }

}
```


```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(name = "user_id")

    private String userId;

    private String password;

    @JsonManagedReference

    @OneToOne(mappedBy = "member")

    private Block block;

}
```


```java
@Entity

@Setter

@Getter

public class Block {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String name;

    @JsonBackReference

    @OneToOne

    private Member member;

}
```


**mappedBy**는 두 테이블이 **서로 관계**가 맺어진 경우

누가 **주인**인지 알려주기위해 사용된다. 

**(mappedBy 속성을 가진 테이블이 가짜주인입니다)**

위의 코드를 보면 **Member** 는 **Block** 테이블을 

**Block**은 **Member** 를 **관계**로 맺고있다.

이러한 관계를 JPA에서는 **양방향관계**라고 보는데

실질적으로 **2개의 단방향**일 뿐이지만 쉽게 부르기 위해 양방향관계라고 말한다.

**member**는 **board_id** **외래키**가 없는 **가짜주인**이고,

**board**는 **member_id** 를 가지고 있는 **진짜주인**이다.

![[JPA34.png]]


![[JPA7.gif]]


이미지 클릭

**가짜주인**은 **조회만** 가능하며, 등록, 수정, 삭제가 불가능하다.

**주인**은 **조회**, **등록**, **수정**, **삭제** 모두 가능하다.

보통 **양방향관계를** 사용하는 이유는

**member** 객체에서도 **조회**가 가능하고

**board** 객체에서도 **조회**가 가능하게 하려할 때 사용한다.

#### > **#7. @OneToMany**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**orphanRemoval**|**true**|false|
|**false**|
|true 인 경우  <br>  해당 필드를 UPDATE 쿼리문으로 null 로 설정할 경우  <br>  관계된 테이블 레코드들은 모두 삭제된다.|
|DELETE 쿼리문과 다른 점은  <br>  DELETE는 해당 레코드를 삭제하면 관계된 테이블의  <br>  레코드들을 모두 삭제하는 반면,  <br>  orphanRemoval 은 해당 테이블의 해당 열이 Null 값 으로   <br>  UPDATE 한 경우, 관계된 테이블의 레코드들을 모두 삭제  <br>  한다.|
|**fetch**|**FetchType.EAGER**   <br>  <br>  관계된 테이블의 정보를 미리 읽어온다  <br>  <br>  join 을 이용한 1개의 SELECT 쿼리문만 실행된다  <br>  <br>  optional 로 out join과 inner join 설정 가능 (기본 out join)|FetchType.LAZY  <br>  (@ManyToOne 는 EAGER)|
|**FetchType.LAZY**  <br>    <br>  실제로 요청하는 순간에만 읽어온다.  <br>  <br>  2개의 SELECT 쿼리문이 실행된다|
|**cascade**|**영속성 전이 기능을 사용한다.**||
|**CascadeType.ALL**  <br>  <br>  CascadeType 5개 전부 사용|
|**CascadeType.PERSIST**  <br>  <br>  INSERT 시 연결된 테이블도 save()|
|**CascadeType.MERGE**  <br>  <br>  UPDATE 시 연결된 테이블도 save()|
|**CascadeType.REMOVE**  <br>  <br>  DELETE 시 연결된 테이블도 delete()|
|**CascadeType.DETACH**|
|**CascadeType.REFRESH**|
|**targetEntity**|**관계를 맺을 Entity Class를 정의한다.**|void.class|
|**mappedBy**|**양방향 관계 설정시 관계의 주체가 되는 쪽에서 정의합니다.**|""|
|**mappedBy="[관계된 테이블의 필드명]"**|
|**mappedBy 를 쓰는 경우는 관계의 주인이 아니기 때문에  <br>  SELECT 역할만 한다.**|
|****주인 테이블은 @ManyToOne 과 @JoinColumn 사용하며  <br>  SELECT, INSERT, UPDATE, DELETE 역할을 한다.****|
|[https://gmlwjd9405.github.io/2019/08/14/bidirectional-association.html](https://gmlwjd9405.github.io/2019/08/14/bidirectional-association.html)|

**[설명]**

**1:N 관계**에서 **1**일 때의 관계를 설정 할 때 사용합니다

List\<Board> boards = new ArrayList\<>();

Set\<Board> boards = new HashMap\<>();

**Member**와 **Board** 에서 예시를 든다면,

**Member**는 여러개의 **Board** 를 작성을 할 수 있기에

**Member** 테이블에 **@OneToMany** 를 사용합니다.

**[코드]**
```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @GetMapping("/members/{memberId}")

    public Member memberDetail(@PathVariable("memberId") Long memberId) {

        return memberRepository.findById(memberId).get();

    }

    @PostMapping("/memberBoardWrite/{userId}")

    public void create(@RequestBody Board board, @PathVariable("userId") Long id) {

        log.info("board -> {}", board);

        log.info("userId -> {}", id);

        Member member =  memberRepository.findById(id).get();

        member.getBoards().add(board);

        memberRepository.save(member);

    }

}
```

```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(name = "user_id")

    private String userId;

    private String password;

    @OneToMany(cascade = CascadeType.ALL)

    private List<Board> boards = new ArrayList<>();

}
```


관계된 테이블에도 저장되게 하기 위해서 **cascade**를 사용했습니다.

(**cascade** 대신 저장하는 **public void create(){...}** 메소드부분에 **@Transactional** 사용하셔 됩니다)

```java
@Entity

@Setter

@Getter

@ToString

public class Board {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String title;

}
```


![[JPA8.gif]]



이미지 클릭

단방향 매핑에서는 **MEMBER_BOARDS** 테이블이

새로 생성되고 **Meber 키**와 **Board** **키**들이 저장됩니다.

(**MEMBER_BOARDS** 테이블에서 키를 관리하게 됨)

**[mappedBy 속성]**

![[JPA35.png]]

**나머지 속성들은 @OneToOne 에서 참고바랍니다**

#### > **#8. @ManyToOne**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**optional**|**false로 설정하면 연관된 엔티티가 항상 있어야 한다.**|true|
|true 는 left outer join 을 사용한다|
|false 는 inner join 을 사용한다|
|**fetch**|**FetchType.EAGER**   <br>  <br>  관계된 테이블의 정보를 미리 읽어온다  <br>  <br>  join 을 이용한 1개의 SELECT 쿼리문만 실행된다  <br>  <br>  optional 로 out join과 inner join 설정 가능 (기본 out join)|FetchType.EAGER  <br>  (@OneToMany 는 LAZY)|
|**FetchType.LAZY**  <br>    <br>  실제로 요청하는 순간에만 읽어온다.  <br>  <br>  2개의 SELECT 쿼리문이 실행된다|
|**cascade**|**영속성 전이 기능을 사용한다.**||
|**CascadeType.ALL**  <br>  <br>  CascadeType 5개 전부 사용|
|**CascadeType.PERSIST**  <br>  <br>  INSERT 시 연결된 테이블도 save()|
|**CascadeType.MERGE**  <br>  <br>  UPDATE 시 연결된 테이블도 save()|
|**CascadeType.REMOVE**  <br>  <br>  DELETE 시 연결된 테이블도 delete()|
|**CascadeType.DETACH**|
|**CascadeType.REFRESH**|
|**targetEntity**|**관계를 맺을 Entity Class를 정의한다.**|void.class|

**나머지 속성들은 @OneToOne 에서 참고바랍니다**

#### > **#9. @OrderBy - Hibernate 패키지**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|해당 필드명으로 **오름차순**으로 정렬  <br>  <br>  **"[필드명] asc"**|"id asc"|
|해당 필드명으로 **내림 차순** 정렬  <br>  <br>  **"[필드명] desc"**|
|**[참고]** 2개이상의 필드명을 정렬 할 때에는 **@OrderBy(value = "title desc, id asc")**|   |   |
|**[참고]** 관계가 설정된 테이블을 정렬 할 때 사용하고, **@OneToMany** 필드에 사용한다   <br>           사용법은 아래 코드를 확인바랍니다|   |   |

**[코드]**
```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @Autowired

    private BoardRepository boardRepository;

    @GetMapping("/members/{memberId}")

    public Member memberDetail(@PathVariable("memberId") Long memberId) {

        return memberRepository.findById(memberId).get();

    }

    @PostMapping("/boards/{userId}")

    public void boardCreate(@RequestBody Board board, @PathVariable("userId") Long userId) {

        Member member = memberRepository.findById(userId).get();

        member.add(board);

        boardRepository.save(board);

    }

    @GetMapping("/members")

    public List<Member> memberList() {

        return memberRepository.findAll();

    }

}
```


```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Column(name = "user_id")

    private String userId;

    private String password;

    @JsonManagedReference

    @OneToMany

    @OrderBy(value = "title desc")

    List<Board> boards = new ArrayList<>();

    public void add(Board board) {

        board.setMember(this);

        boards.add(board);

    }

}
```



```java
@Entity

@Setter

@Getter

@ToString

public class Board {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String title;

    @JsonBackReference

    @ManyToOne

    private Member member;

}
```






**[SQL]**

![[JPA36.png]]

> ~~**#10. @ManyToMany**~~

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
||||

> **#11. @Id (단일 기본키)**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

![[JPA37.png]]

Primary Key = id

**[설명]**

**해당 테이블**에서 **해당 필드명**을 **기본키**로 설정합니다.

**[참고]**

**@Id 어노테이션**을 여러 필드에 사용한다고 해서 **복합키**로 설정은 되지 않습니다.

아래의 **@EmbeddedId** 와 **@IdClass** 를 이용하여 **복합키**를 설정합니다

> **#12. @EmbeddedId (복합 기본키)**

|   |   |   |
|---|---|---|
|**속성**|**기능**|기본값|
|**없음**|   |   |

**[코드]**

```java
@Entity

@Getter

@Setter

@ToString

public class Seat {

    @EmbeddedId

    private SeatId seatId;

    private String type;

}
```


```java
@Embeddable

@Getter

public class SeatId implements Serializable {

    private Long x;

    private Long y;

}
```


```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface SeatRepository extends JpaRepository<Seat, SeatId> {

}
```


id 타입이 바뀌어 Long 아닌 SeatId 로 바꿔주어여 한다.

**[SQL]**


![[JPA38.png]]

**[테이블]**

![[JPA39.png]]

**[기타 참고]**

 [Legacy DB의 JPA Entity Mapping (복합키 매핑 편) - 우아한형제들 기술 블로그

안녕하세요. 우아한형제들에서 배달의민족 서비스의 광고시스템을 개발하고 있습니다. 시스템을 점진적으로 Spring Boot / JPA 기반으로 이관하면서 경험했던 내용을 공유하고자 합니다.

woowabros.github.io](https://woowabros.github.io/experience/2019/01/04/composit-key-jpa.html)

> **#13. @IdClass (복합 기본키)**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**[코드]**
```java
@Entity

@Getter

@Setter

@ToString

@IdClass(value = SeatId.class)

public class Seat {

    @Id

    private Long x;

    @Id

    private Long y;

    private String type;

}
```


```java
@EqualsAndHashCode(onlyExplicitlyIncluded = true)

@Getter

@NoArgsConstructor

@AllArgsConstructor

public class SeatId implements Serializable {

    @EqualsAndHashCode.Include

    private Long x;

    @EqualsAndHashCode.Include

    private Long y;

}
```


```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface SeatRepository extends JpaRepository<Seat, SeatId> {

}
```


**[SQL]**

![[JPA40.png]]

**[테이블]**

![[JPA41.png]]

**[기타 참고]**

 [Legacy DB의 JPA Entity Mapping (복합키 매핑 편) - 우아한형제들 기술 블로그](https://woowabros.github.io/experience/2019/01/04/composit-key-jpa.html)

> **#14. @GeneratedValue**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**strategy**|**GenerationType.AUTO**  특정 DB에 맞게 자동 선택|**GenerationType.AUTO**|
|**GenerationType.IDENTITY**  DB의 identity 컬럼을 이용|
|**GenerationType.SEQUENCE**  DB의 시퀀스 컬럼을 이용|
|**GenerationType.TABLE**  유일성이 보장된 데이터베이스 테이블을 이용|
|**generator**|Oracle 데이터베이스의 시퀀스 이름을 참조할 때 사용||
|**@SequenceGenerator**(  <br>        name="S_MY_SEQ",  <br>        sequenceName="SEQ_NO"  <br>   )  <br>  **@GeneratedValue**(  <br>         strategy=GenerationType.SEQUENCE,  <br>         **generator**="S_MY_SEQ"  <br>   )|""|

> **#15. @Embedded**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**[코드]**

```java
@Entity

@Getter

@Setter

@ToString

public class Delivery {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Embedded

    private Address address;

}
```


```java
@Embeddable

public class Address {

    private String zoneCode;

    private String address;

    private String buildingName;

}
```


**[설명]**

**@****Embedded** 와 **@Embeddable** 은 항상 같이 사용되어야 한다.

**@****Embedded** 를 사용하는 이유는 **객체지향적 설계**를 하기위해 필요하기 때문이다

객체의 필드가 많아질 수록 **가독성**이 떨어지기 때문에

같이 묶을 수 있는 것들을 객체로 만들어 **객체지향적**으로 **설계**하기 위함이다.

**[테이블]**

![[JPA42.png]]

Delivery 테이블

> **#16. @Embeddable**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**@Embedded 참고 바람**

**@EmbeddedId, @Embedded 와 같이 사용된다.**

> **#17. @Enumerated**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|EnumType.ORDINAL : enum 순서를 값으로 DB에 저장|EnumType.ORDINAL|
|EnumType.STRING : enum 이름을 값으로 DB에 저장|

**[코드]**

```java
@Entity

@Getter

@Setter

@ToString

public class Message {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String message;

    @Enumerated(value = EnumType.STRING)

    private MessageType type;

}
```


```java
public enum  MessageType {

    EMAIL, SMS, KAKAO;

}
```


**[value 속성]**

**- EnumType.ORDINAL**

![[JPA43.png]]

EnumType.ORDINAL

**- EnumType.STRING**

![[JPA44.png]]

EnumType.STRING

> **#18. @Transient**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**DB**에 반영이 안되는 값을 **임시**로 **저장** 할 수 있는 **필드**이다

단방향 매핑관계에서 양방향 관계처럼

관계된 테이블에서도 json 으로 볼 수 있게끔 사용하기도 한다.

**@Transient List\<?> list = new ArrayList<>();**

> **#19.@AttributeOverride**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|name= "@Embedded 의 필드명"||
|**column**|column= @Column 재정의||

**[코드]**

```java
@Entity

@Getter

@Setter

@ToString

public class Delivery {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Embedded

    @AttributeOverride(name = "zoneCode", column = @Column(name = "first_address"))

    private Address address;

}
```



```java
@Embeddable

@ToString

@Setter

@Getter

public class Address {

    private String zoneCode;

    private String address;

    private String buildingName;

}
```



**[설명]**

**@Embeddable** 클래스의 1개의 **@Column** 을 재정의 할 때 사용한다.

**name** 은 **@Embeddable** 클래스의 재정의 할 **필드명(멤버변수)**

**Column** 은 **@Column** 이다

**[참고 1]**

**zoneCode** 는 **Address 클래스**의 **필드명** 중 하나이고,

**first_address** 는 재정의될 **테이블 컬럼명**이다.

**@Embedded** 는 

 * @see Embeddable   
 * @see AttributeOverride   
 * @see AttributeOverrides   
 * @see AssociationOverride   
 * @see AssociationOverrides

구성 되어있기 때문에 생략해도 DB에 저장은 된다.

**[참고 2]**

**@Embeddable** 클래스에

**@Setter** 가 없는 경우 값이 **null** 로 들어갑니다

```json
// JSON Request

{

  "address": {

    "zoneCode": "06120",

    "address": "서울 강남구 강남대로 480",

    "buildingName": "올리브영"

  }

}
```

**[테이블]**


![[JPA45.png]]

**zone_code** 에서 **first_address** 로 재정의 된 모습

> **#20. @AttributeOverrides**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|여러개의 @AttributeOverride 사용  <br>   <br>  {  <br>    @AttributeOverride,  <br>    @AttributeOverride  }|[]|

**[코드]**

```java
@Getter

@Setter

@ToString

public class Delivery {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    @Embedded

    @AttributeOverrides(value = {

            @AttributeOverride(name = "zoneCode", column = @Column(name = "address1")),

            @AttributeOverride(name = "address", column = @Column(name = "address2")),

            @AttributeOverride(name = "buildingName", column = @Column(name = "address3"))

    })

    private Address address;

}
```

```java
@Embeddable

@ToString

@Setter

@Getter

public class Address {

    private String zoneCode;

    private String address;

    private String buildingName;

}
```

**[테이블]**

![[JPA46.png]]

**[설명]**

****@Embeddable****클래스의 **여러개**의 **@Column**을 **재정의** 할 때 사용한다.

**@AttributeOverride** 속성은 위에서 참고 바람

**[참고]**

**@Embeddable** 클래스에

**@Setter** 가 없는 경우 값이 **null** 로 들어갑니다

```json
// JSON Request

{

  "address": {

    "zoneCode": "06120",

    "address": "서울 강남구 강남대로 480",

    "buildingName": "올리브영"

  }

}
```

#### > **#21. @Temporal**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|TemporalType.DATE : 날짜. 데이터베이스 date 타입과 맵핑|필수|
|TemporalType.TIME : 시간. 데이터베이스 time 타입과 맵핑|
|TemporalType.TIMESTAMP : 날짜와 시간. 데이터베이  <br>  timestamp 타입과 맵핑|

#### > **#22. @CreationTimestamp**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**[코드]**

```java
@Entity

@ToString

@Getter

public class LoginLog {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    @CreationTimestamp

    private LocalDate createAt;

}
```

**[설명]**

**생성일**, **작성날짜**를 위해 사용된다.

따로 값을 보내주지 않아도 **INSERT** 시 자동으로 값을 넣어준다

**[참고]**

**@RequestBody** 어노테이션에 **@Entity** 클래스를 사용하는 경우 적절하지 않다.

다른 값도 바뀔 수 있는 염려가 있기에 **LoginLogDto** 클래스를 하나 더 만들어서

바꿀 필드만 적는 것이 좋다

![[JPA47.png]]

위 그림과 같이 할 시 문제점은 아래 gif 참고 바람

![[JPA9.gif]]

이미지 클릭

처음에 **INSERT** 될 시에는 아무런 문제가 없지만

2번째 **UPDATE** 될 시, **createAt** 값이 **null** 로 변했다.

이유는 **사용자**가 **요청**시(request) **createAt** 값을 보내지 않았기 때문에

**null** 로 업데이트 된 것이다.

**@CreationTimestamp** 은 **INSERT** 시 값을 저장해준다.

**@UpdateTimestamp** 는 **INSERT** 와 **UPDATE** 시 값을 저장해준다.

때문에 **@UpdateTimestamp** 는 **null** 값으로 들어와도 문제가 없다

> **#23. @UpdateTimestamp**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**[코드]**


```java
@Entity

@ToString

@Getter

public class LoginLog {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    @UpdateTimestamp

    private LocalDate updateAt;

}
```

**[설명]**

**변경일**, **수정일**에 사용된다

따로 값을 보내주지 않아도 **INSERT, UPDATE** 시 자동으로 값을 넣어준다

#### > **#24. @Lob**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**없음**|   |   |

**[코드]**

```java
@Entity

@Setter

@Getter

@ToString

public class Board {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String title;

    @Lob

    private String content;

}
```

**[SQL]**


![[JPA48.png]]
**[설명]**

**JPA** 에서 **String** 은 DB에서는 **기본 varchar(255)** 으로 테이블이 만들어진다.

**varchar**의 최대 사이즈는 **32,672 byte** (약 20,000 글자)

**clob** 의 최대 사이즈는 **2 Gb** 입니다. (byte 와 61,214배 차이)

많은 내용을 저장하기 위해서는 **@Lob** 을 사용한다.

또는 **@Column** 에서 타입지정

![[JPA49.png]]

대용량 텍스트

> **#25. @Query**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|JPQL 쿼리문 작성|""|
|**countQuery**|모름..|""|
|**countProjection**|모름..|""|
|**nativeQuery**|true = DB 쿼리문으로 작성|false|
|false = JPQL 쿼리문으로 작성|
|**name**|모름..|""|
|**countName**|모름..|""|

**[코드]**

```java
@RestController

@Slf4j

public class api {

    @Autowired

    private MemberRepository memberRepository;

    @GetMapping("/member")

    public Member memberDetail(String userId) {

        return memberRepository.selectUserId(userId);

    }

}
```


```java
@Entity

@Getter

@ToString

@Setter

public class Member {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String userId;

    private String password;

}
```

```java
public interface MemberRepository extends JpaRepository<Member, Long> {

    // JPQL argument 방식

    @Query(value = "select mem from Member mem where mem.userId= ?1")

    Member selectUserId(String userId);

    // JPQL @Param 방식

    @Query(value = "select mem from Member mem where mem.userId= :userName")

    Member selectUserId2(@Param("userName") String userId);

    // Native Query @Param 방식

    @Query(value = "select * from member where user_id =:userName", nativeQuery = true)

    Member selectUserId3(@Param("userName") String userId);

    // 위 3개 모두 똑같은 쿼리문입니다.

}
```

**[설명]**

**JPQL 쿼리문**을 작성할 때 사용합니다.

**[참고]**

![[JPA50.png]]

정상

> **#26. @Modifying**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**flushAutomatically**|모름..|false|
|**clearAutomatically**|모름..|false|

**[설명]**

DD이 Annotation은 @Query Annotation으로 작성 된 변경, 삭제 쿼리 메서드를 사용할 때 필요합니다.

 즉, 조회 쿼리를 제외하고, 데이터에 변경이 일어나는 INSERT, UPDATE, DELETE, DDL 에서 사용합니다.

 주로 벌크 연산 시에 사용됩니다.

**[참고]**

 [Spring Data JPA @Modifying (1) - clearAutomatically

이 글을 작성하게 된 계기는 Spring Data JPA의 @Modifying에 있는 flushAutomatically에 대해 의문점이 생겼고, 그에 대한 학습 테스트를 해보면서 였습니다. 하지만 @Modifying의 Attribute가 clearAutomaticall..

devhyogeon.tistory.com](https://devhyogeon.tistory.com/4)

> **#27. @ColumnDefault**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
||||

> **#28. @Where**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
||||

> **#29. @DynamicUpdate**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|모름..|true|

**[코드]**

```java
@RestController

@Slf4j

public class api {

    @Autowired

    private GoodRepository goodRepository;

    @PostMapping("/good")

    public void goodCreate(@RequestBody Good good) {

        log.info("good -> {}", good);

        goodRepository.save(good);

    }

}
```


```java
@Entity

@ToString

@Getter

@DynamicUpdate

public class Good {

    @Id

    @GeneratedValue(strategy = GenerationType.IDENTITY)

    private Long id;

    private String name;

    private Long price;

}
```










**[요약]**




|  |  |  |
| ---- | ---- | ---- |
| **@DynamicUpdate 미적용** | price를 3000 으로 수정한 경우 | ![[JPA51.png]] |
| name을 book 으로  <br>price 를 5000 으로 수정한 경우 | ![[JPA52.png]] |  |
| **@DynamicUpdate 적용** | price를 3000 으로 수정한 경우 | ![[JPA53.png]] |
| name을 book 으로  <br>price 를 5000 으로 수정한 경우 | ![[JPA54.png]] |  |

**[설명]**

**@DynamicUpdate** 는 **SELECT** 로 **조회** 후

**조회 한 객체**와 **save() 하려는 객체**와 **비교**해서

**변경된 필드만**을 **쿼리문의 인자**로 **생성**한다.

**[참고]**

![[Images/JPA/JPA55.png]]

[출처 - Baeldung] https://www.baeldung.com/spring-data-jpa-dynamicupdate

> **#30. @DynamicInsert**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
||||

> **#31. @SequenceGenerator**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|식별자 생성기 이름|필수|
|**sequenceName**|DB에 등록되어 있는 시퀀스 이름|hibernate_sequence|
|**initialValue**|DDL 생성 시에만 사용 됨. 시퀀스 DDL을 생성할 때 처음 시작하는 수|1|
|**allocationSize**|시퀀스 한 번 호출에 증가하는 수|50|
|**catalog, schema**|DB catalog, schema 이름||

> **#32. @TableGenerator**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**name**|식별자 생성기 이름|필수|
|**table**|키생성 테이블명|hibernate_sequence|
|**pkColumnName**|시퀀스 컬럼명|squence_name|
|**valueColumnName**|시퀀스 값 컬럼명|next_val|
|**pkColumnValue**|키로 사용할 값 이름|엔티티 이름 (=클래스명)|
|**initialValue**|초기 값. 마지막으로 생성된 값이 기준|0|
|**allocationSize**|시퀀스 한 번 호출에 증가하는 수|50|
|**uniqueContraints**|유니크 제약 조건을 지정||
|**catalog, schema**|DB catalog, schema 이름||

> **#33. @EntityListeners(AuditingEntityListener.class)**

|   |   |   |
|---|---|---|
|**속성**|**기능**|**기본값**|
|**value**|중복 필드 처리하는 상위 클래스|[]|

**[코드]**

```java
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class AbstractEntity extends AbstractEntityId {

    @CreationTimestamp
    @Column(name = "createdAt", nullable = false, updatable = false)
    private Date createdAt;

    @UpdateTimestamp
    @Column(name = "updatedAt", nullable = false)
    private Date updatedAt;
}

@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
abstract class AbstractEntityId implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

}
```

**[설명]**

id 만 상속하고자 한다면, AbstractEntityId 를 상속시키면 되고,

createdAt, 과 updatedAt 도 상속시키고자 한다면 AbstractEntityId 를 상속받은 AbstractEntity 를 상속시키면 된다.

(하나의 클래스에 id, creadtedAt, updatedAt 을 사용할 수 있지만,

createdAt 과 updatedAt 을 사용하지 않는 경우도 있기에 분리하여 사용한다)

**이 방법 이외에도 리스너 클래스를 생성하여 유동적으로 엔티티를 관리할 수 있다. (아래 사이트 참고)**

 데이터 변경 알림 - @EntityListeners

spring 의 data-jpa 사용시 데이터 변경시 알림을 받는 방법이 있다. EntityListener 클래스를 만들고 



erim1005.tistory.com](https://erim1005.tistory.com/entry/%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B3%80%EA%B2%BD-%EC%95%8C%EB%A6%BC-EntityListeners)

> **# 쿼리 메소드 (queryMethod)**

|   |   |
|---|---|
|**기본 제공**|   |
|**save()**|레코드 저장(insert, update)|
|**findAll()**|전체 레코드 불러오기 - 정렬(sort), 페이징(pageable) 가능|
|**count()**|레코드 갯수|
|**delete()**|레코드 삭제|

|   |   |
|---|---|
|**메소드**|**쿼리문**|
|**findById(Long id);**|SELECT * FROM [테이블명] WHERE id= ?|

Member findBy();  
List\<Member> findAllBy();  
Member findFirstBy();  
Member findDistinctFirstBy();  
Member findTopBy();  
Member findDistinctTopBy();  
Member findDistinctBy();  
Member findMemberBy();  
Member findMembersBy();  
  
Member deleteBy();  
List\<Member> deleteAllBy();  
Member deleteDistinctBy();  
Member deleteMemberBy();  
Member deleteMembersBy();  
  
Member countBy();  
List\<Member> countAllBy();  
Member countDistinctBy();  
Member countMemberBy();  
Member countMembersBy();  
  
boolean existsBy();  
boolean existsAllBy();  
boolean existsDistinctBy();  
boolean existsMemberBy();  
boolean existsMembersBy();  
  
Member getBy();  
List\<Member> getAllBy();  
Member getTopBy();  
Member getDistinctTopBy();  
Member getFirstBy();  
Member getDistinctFirstBy();  
Member getDistinctBy();  
Member getMemberBy();  
Member getMembersBy();  
  
Member queryBy();  
List\<Member> queryAllBy();  
Member queryTopBy();  
Member queryDistinctTopBy();  
Member queryFirstBy();  
Member queryDistinctFirstBy();  
Member queryDistinctBy();  
Member queryMemberBy();  
Member queryMembersBy();  
  
Member readBy();  
List\<Member> readAllBy();  
Member readTopBy();  
Member readDistinctTopBy();  
Member readFirstBy();  
Member readDistinctFirstBy();  
Member readDistinctBy();  
Member readMemberBy();  
Member readMembersBy();  
  
Member removeBy();  
Member removeAllBy();  
Member removeDistinctBy();  
Member removeMemberBy();  
Member removeMembersBy();  
  
Member streamBy();  
List\<Member> streamAllBy();  
Member streamTopBy();  
Member streamDistinctTopBy();  
Member streamFirstBy();  
Member streamDistinctFirstBy();  
Member streamDistinctBy();  
Member streamMemberBy();  
Member streamMembersBy();

> **# 오류**

**[오류]**

**org.hibernate.InstantiationException: No default constructor for entity:**

**[코드]**

**[Entity 클래스]**.repository.findById(**[Long id]**).orElse(null);

**[원인]**

해당 @Entity 클래스에 기본생성자(default Constructor) 가 없기 때문

**[해결]**

**기본생성자**를 작성하거나, **@NoArgsConstructor** 작성하면 된다

---

**[오류]**

**org.springframework.beans.factory.BeanCreationException: Error creating bean with name ... defined in @EnableJpaRepositories declared on JpaRepositoriesRegistrar.EnableJpaRepositoriesConfiguration:**

****[코드]****

(OneToMany 클래스에서)

**[Entity 클래스]**.findBy**[필드명]**(String **[필드명]**);

**[원인]**

repository 인터페이스가 존재 하지 않음

**[해결]**

JpaRespository 클래스가 extend 된 해당 **[Entity]**Repository 클래스 생성

---

**[오류]**

**Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'personRepository': FactoryBean threw exception on object creation; nested exception is java.lang.IllegalArgumentException: Validation failed for query for method public abstract java.util.List com.example.demo.chapter_2.repository.PersonRepository.findByMonthOfBirthDay(int)!**

**[원인]**

![[JPA56.png]]

**JPQL (nativeQuery=false) 엔티티 쿼리문에서는 SELECT * 이 안되기 때문에**

**PERSON 클래스를 Alias 한 값을 써야한다.**

**SELECT person FROM Person person WHERE person.birthday.month = ?1**

**[해결]**

![[JPA57.png]]

---

**[오류]**

```
2020-09-20 06:13:49.399 ERROR 17300 --- [nio-8080-exec-1] o.a.c.c.C.[.[.[/].[dispatcherServlet]
: Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception
[Request processing failed; nested exception is org.springframework.http.converter
.HttpMessageConversionException: Type definition error: [simple type, class org.hibernate
.proxy.pojo.bytebuddy.ByteBuddyInterceptor]; nested exception is com.fasterxml.jackson.databind
.exc.InvalidDefinitionException: No serializer found for class org.hibernate.proxy.pojo
.bytebuddy.ByteBuddyInterceptor and no properties discovered to create BeanSerializer 
(to avoid exception, disable SerializationFeature.FAIL_ON_EMPTY_BEANS) (through reference
chain: java.util.HashMap["teams"]->java.util.ArrayList[0]->com.vuebook.taskagile.domain.model
.team.Team["user"]->com.vuebook.taskagile.domain.model.user.User$HibernateProxy$skNn6Vvs
["hibernateLazyInitializer"])] with root cause
```

```
[] 경로의 컨텍스트에서 [dispatcherServlet] 서블릿에 대한 Servlet.service ()에서 예외가 발생했습니다.
[요청 처리에 실패했습니다. 중첩 된 예외는 org.springframework.http.converter.HttpMessageConversion
Exception : 유형 정의 오류 : [단순 유형, 클래스 org.hibernate.proxy.pojo.bytebuddy.ByteBuddyInterce
ptor]; 중첩 된 예외는 com.fasterxml.jackson.databind.exc.InvalidDefinitionException : org.hiberna
te.proxy.pojo.bytebuddy.ByteBuddyInterceptor 클래스에 대한 serializer가 없으며 BeanSerializer를 
생성하는 속성이 없습니다 (예외를 방지하려면 SerializationFeature.FAIL_ON_EMPTY_BEANS를 비활성화) 참조
체인 : java.util.HashMap [ "teams"]-> java.util.ArrayList [0]-> com.vuebook.taskagile.domain.
model.team.Team [ "user"]-> com.vuebook.taskagile .domain.model.user.User $ HibernateProxy 
$ skNn6Vvs [ "hibernateLazyInitializer"])] 근본 원인 포함
```

**[원인]**

**Lazy 로딩으로 인한 오류**

```java
@Entity
@Getter
@ToString
@NoArgsConstructor
@EqualsAndHashCode(of = {"id"})
public class User implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;

    private String emailAddress;

    private String password;

    private String firstName;

    private String lastName;

    @CreationTimestamp
    private Date createAt;

    @Builder
    private User(String username, String emailAddress, String password, String firstName, String lastName) {
        this.username = username;
        this.emailAddress = emailAddress;
        this.password = password;
        this.firstName = firstName;
        this.lastName = lastName;
    }

}
```

```java
@Entity
@Getter
@ToString
@NoArgsConstructor
@EqualsAndHashCode(of = {"id"})
public class Team implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private boolean archived;

    @CreationTimestamp
    private Date createAt;

    @ManyToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    private User user;

    @Builder
    private Team(String name, User user) {
        this.name = name;
        this.user = user;
    }
}
```

```java
@Slf4j
@RestController
@RequiredArgsConstructor
@RequestMapping(value = "/api/me")
public class MeApiController {

    private final TeamService teamService;
    private final BoardService boardService;


    @GetMapping
    public Map<String, List<?>> getMyData(Long userId) {

        log.info("userId -> {}", userId);

        List<Team> teams = teamService.findTeamsByUserId(userId);
        List<Board> boards = boardService.findBoardsByMembership(userId);

        Map<String, List<?>> maps = new HashMap<>();
        maps.put("teams", teams);
        maps.put("boards", boards);

        return maps;
    }
}
```

**[해결]**

![[JPA58.png]]

> **# 양방향 매핑 참고**

DB에는 참조키(foreign key)가 있기 때문에 양방향관계인 반면,

객체에서는 A->B 단방향과 B->A 단뱡향을 

쉽게 부르기 위해 양방향관계라고 말할 뿐 그냥 단방향관계이다. 

**(객체에서는 양방향관계라는 표현이 없다)**

JPA 를 처음에 설계를 할 때는 단방향으로 설계 한 뒤

양방향 관계가 필요 할 때 추가해주는 것이 설계하기 편한다.

(양방향 까지 다 설계하면 어디서 오류나는지 찾기가 힘들기 때문)


---
참조 - https://velog.io/@ttomy/SpringBootJPA-%EA%B3%B5%EA%B3%B5%EB%8D%B0%EC%9D%B4%ED%84%B0-db%EC%A0%80%EC%9E%A5%EA%B3%BC-API%EB%A7%8C%EB%93%A4%EA%B8%B0


https://riverblue.tistory.com/47

https://theheydaze.tistory.com/196


https://code-lab1.tistory.com/288