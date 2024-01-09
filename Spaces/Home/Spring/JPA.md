
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

#### > **#1. @Entity**

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


#### > **#2. @Table**

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

#### > **#3. @Column**


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


#### > **#4. @JoinColumn**

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

#### > **#5. @JoinColumns**


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


















---
참조 - https://velog.io/@ttomy/SpringBootJPA-%EA%B3%B5%EA%B3%B5%EB%8D%B0%EC%9D%B4%ED%84%B0-db%EC%A0%80%EC%9E%A5%EA%B3%BC-API%EB%A7%8C%EB%93%A4%EA%B8%B0


https://riverblue.tistory.com/47

https://theheydaze.tistory.com/196

