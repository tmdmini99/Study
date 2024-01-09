
###  **JPA**

Java Persistence Api 이름처럼 **자바 영속성에 관한 api**로 orm에 대한 인터페이스라고 생각하면된다. 사실 JPA는 **표준 명세**를 말하는것이며 RDS데이터를 매핑시킬 객체를 Entity라고 하는데, JPA의 핵심은 이 Enity를 관리하는 **EntityManager**라고 한다. 그러므로 실제로 **JPA를 사용하기위해선 Hibernate와 같이 JPA의 EntityManager를 구현한(Hibernate, DataNucleus, EclipseLink 등) 구현체**를 사용해야한다

![[JPA.png]]








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
|****@ManyToOne****|테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 다대일(N:1)의 관계를 맵핑할 때  <br>  설정한다.|
|****@ManyToMany****|테이블 연관관계를 맵핑할 때 다중성을 나타내는  <br>  설정값으로 이용되며 다:다(N:N)의 관계를 맵핑할 때  <br>  설정한다.|
|**@OrderBy -** Hibernate 패키지|해당 열 기준으로 오름차순 내림차순 정렬할 때 사용  <br>  JPA 쿼리문 뒤에 OrderBy 절이 붙게 됨|
|**@Id**|테이블에서 Identity 키로 사용할 필드를 지정|
|****@EmbeddedId****|복합키를 사용할 때 사용 (2개이상의 PK키)|
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

> **#1. @Entity**

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


> **#2. @Table**

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

> **#3. @Column**


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

![[Pasted image 20240109141117.png]]









---
참조 - https://velog.io/@ttomy/SpringBootJPA-%EA%B3%B5%EA%B3%B5%EB%8D%B0%EC%9D%B4%ED%84%B0-db%EC%A0%80%EC%9E%A5%EA%B3%BC-API%EB%A7%8C%EB%93%A4%EA%B8%B0


https://riverblue.tistory.com/47

https://theheydaze.tistory.com/196

