## JPQL

JPQL 을 사용하려면 interface로 만들어야함


1. JPQL 쿼리 문자열 작성: JPQL 쿼리는 엔티티와 엔티티의 속성을 기반으로 작성됩니다. SELECT, FROM, WHERE 등의 키워드를 사용하여 원하는 데이터를 조회하거나 조건을 설정할 수 있습니다.
    
2. 쿼리 실행: 작성된 JPQL 쿼리를 JPA의 `createQuery()` 메서드를 통해 실행할 수 있습니다. 실행 결과로는 `Query` 객체가 반환됩니다.
    
3. 결과 처리: `Query` 객체를 사용하여 결과를 가져올 수 있습니다. `getResultList()` 메서드를 사용하면 결과를 리스트 형태로 반환받을 수 있고, `getSingleResult()` 메서드를 사용하면 단일 결과를 반환받을 수 있습니다.


```java
//List 형태로 반환
em.createQuery(jpql, OpenApiJpaEntity.class).getResultList(); 

// 단일 형태로 반환
em.createQuery(jpql, OpenApiJpaEntity.class).getSingleResult();

```

### 기본 문법
####  SELECT 문
---


**> 대소문자 구분**

- 엔티티와 속성은 대소문자 구분

- JPQL 키워드는 구분하지 않음

**> 엔티티 이름**

- 테이블 명 대신 엔티티 명을 사용, @Entity(name=" ") 으로 설정 가능

- 지정하지 않을 시 클래스 명을 기본값으로 사용(기본값을 추천)

**> 별칭은 필수**

- JPQL은 별칭을 필수

- AS는 생략 가능



#### TypeQuery, Query

---

  

JPQL 실행 시 쿼리 객체 생성이 필요

  

ㅇ TypeQuery : 반환할 타입을 명확하게 지정할 수 있을 경우

```java
TypedQuery<Member> query = 

    em.createQuery("SELECT m FROM Member m", Member.class);

                                            // 반환이 명확

List<Member> resultList = query.getResultList();

for (Member member : resultList) {

    System.out.println("member = " + member);

}
```

ㅇ Query : 반환 타입을 명확하게 지정할 수 없을 경우

,여러 엔티티나 컬럼을 선택할 경우(반환 타입이 명확하지 않을 경우) 사용

```java
Query query = 

    em.createQuery("SELECT m.username, m.age FROM Member m");

List resultList = query.getResultList();

for (Object o : resultList) {

    Object[] result = (Object[]) o; // 결과가 둘 이상일 경우 Object[]

    System.out.println("username = " + result[0]);

    System.out.println("age = " + result[1]);

}
```


#### 결과 조회
---
ㅇ query.getResultList() : 결과를 예제로 반환

- 결과가 없을 경우 빈 컬렉션 반환

ㅇ query.getSingleResult() : 결과가 정확히 하나일 때 사용

- 결과가 없으면 javax.persistence.NoResultException 예외 발생

- 결과가 1보다 많으면 javax.persistence.NonUniqueResultException 예외 발생

### 파라미터 바인딩

---

ㅇ 이름 기준 파라미터(Named parameters)

- 파라미터를 이름으로 구분

- : 사용

- 더 명확한 방식

```java
String usernameParam = "User1";

TypedQuery<Member> query = 

    em.createQuery("SELECT m FROM Member m where m.username = :username", Member.class);

                                                            // 파라미터 정의

query.setParameter("username", usernameParam); // 파라미터 바인딩

List<Member> resultList = query.getResultList();

// 아래와 같이 작성 가능(메소드 체인)

List<Member> members = 

    em.createQuery("SELECT m FROM Member m where m.username = :username", Member.class)

    .setParameter("username", usernameParam)

    .getResultList();

```


ㅇ 위치 기준 파라미터(Positional parameters)

- ? 다음에 위치 값을 지정



```java
List<Member> member =

    em.createQuery("SELECT m FROM Member m where m.username = ?1", Member.class)

    .setParameter(1, usernameParam)

    .getResultList();

```




### 프로젝션(projection)

---

- SELECT 절에 조회할 대상을 지정하는 것.

.엔티티

.엠비디드 타입

.스칼라 타입(숫자, 문자 등 기본 타입)

  

#### 엔티티 프로젝션

---
- 원하는 객체를 바로 조회

- 엔티티 프로젝션으로 조회한 엔티티는 영속성 컨텍스트에서 관리

```java
SELECT m FROM Member m        // 회원

SELECT m.team FROM Member m // 팀
```

#### 임베디드 타입 프로젝션

---


- 엔티티와 거의 비슷하게 사용

- 조회의 시작점이 될 수 없음

- 엔티티 타입이 아닌 값 타입. 직접 조회한 임베디드 타입은 영속성 컨텍스트에서 관리되지 않음


```java
String query = "SELECT o.address FROM Order o";

List<Address> addresses = em.createQuery(query, Address.class)

                            .getResultList();
```


```java
select

    order.city,

    order.street,

    order.zipcode

from

    Orders order
```


#### 스칼라 타입 프로젝션

---

- 숫자, 문자, 날짜와 같은 기본 데이터 타입

```java
List<String> username = 

    em.createQuery("SELECT username FROM Member m", String.class)

      .getResultList();

```

#### 여러 값 조회

---

여러 값 선택 시 Query 사용


- 여러 프로젝션

```java
Query query = 

    em.createQuery("SELECT m.username, m.age, FROM Member m");

List resultList = query.getResultList();

Iterator iterator = resultList.iterator();

while (iterator.hasNext()) {

    Object[] row = (Object[]) iterator.next();

    String username = (String) row[0];

    Integer age = (Integer) row[1];

}
```



- 여러 프로젝션 Object[] 조회
```java
List<Object[]> resultList = 

    em.createQuery("SELECT m.username, m.age FROM Member m")

      .getResultList();

for (Object[] row : resultList) {

    String username = (String) row[0];

    Integer age = (Integer) row[1];

}


```

- 여러 프로젝션 엔티티 타입 조회

```java
List<Object[]> resultList = 

    em.createQuery("SELECT o.member, o.product, o.orderAmount FROM Order o")

      .getResultList();

for (Object[] row : resultList) {

    Member member = (Member) row[0];    // 엔티티

    Product product = (Product) row[1];    // 엔티티

    int orderAmount = (Integer) row[2];    // 스칼라

}

```


#### New 명령어

---

- SELECT 다음 NEW 명령어 사용 시 반환받을 클래스 지정 가능,

  이 클래스의 생성자에 JPQL 조회 결과를 넘겨줄 수 있음

- New 명령어를 사용한 클래스로 TypeQuery 사용이 가능하여 객체 변환 작업에 효율적

> 명령어 사용 시 주의사항

1. 패지키 명을 포함한 전체 클래스 명 기입

2. 순서와 타입이 일치하는 생성자 필요

```java
public class UserDTO {

    private String username;

    private int age;

    public UserDTO(String username, int age) {

        this.username = username;

        this.age = age;

    }

    //...

}

TypeQuery<UserDTO> query =

    em.createQuery("SELECT new test.jpql.UserDTO(m.username, m.age)    

                    FROM Member m", UserDTO.class);

List<UserDTO> resultList = query.getResultList();

```

### 페이징 API

- setFirstResult (int startPosition) : 조회 시작 위치(zero base)
- setMaxResults (int maxResult) : 조회할 데이터 수
- 데이터 방언(Dialect) 덕분에 DB마다 다른 페이징 처리를 같은 API로 처리

```java
TypeQuery<Member> query = 

    em.createQuery("SELECT m FROM Member m ORDER BY m.username DESC",

                    Member.class);

// 11~30번 데이터 조회

query.setFirstResult(10);    // 조회 시작 위치

query.setMaxResults(20);    // 조회할 데이터 수

query.getResultList();
```


### 집합과 정렬

---

  

#### 집합 함수

---

  

 COUNT

 MAX, MIN

 AVG

 SUM

  

 > 참고 사항

- null 값은 무시

- 값이 없는데 집합 함수 사용 시 null. 단, count는 0

- DISTINCT를 COUNT에서 사용 시 임베디드 타입은 지원 X

  

#### GROUP BY, HAVING

---

  

- 통계 데이터를 구할 때 특정 그룹끼리 묶어줌.

- 보통 전체 데이터를 기준으로 처리하므로 실시간으로 사용하기에 부담

- 결과가 아주 많을 경우 통계 결과만 저장하는 테이블을 별도로 만들어 두고 사용자가 적은 새벽에 통계 쿼리를 실행

  

#### 정렬(ORDER BY)

---


- 결과 정렬 시 사용

  
### JPQL 조인

---

#### 내부 조인

---
- INNER JOIN 사용

- INNER는 생략 가능

```java
String teamName = "teamA";

String query = "SELECT m FROM Member m INNER JOIN m.team t"

                + "WHERE t.name = :teamName";

List<Member> members = em.createQuery(query, Member.class)

    .setParameter("teamName", teamName)

    .getResultList();


```

- 서로 다른 타입의 두 엔티티 조회 시

```java
List<Object[]> result = em.createQuery("SELECT m, t FROM Member m JOIN m.team t")

                          .getResultList();

for (Object[] row : result) {

    Member member = (Member) row[0];

    Team team = (Team) row[1];

}
```


#### 외부 조인
```java
SELECT m 

FROM Member m LEFT [OUTER] JOIN m.team t
```


#### 컬렉션 조인
- 일대다 관계나 다대다 관계처럼 컬렉션을 사용하는 곳에 조인하는 것.

* 다대일 조인 : 단일 값 연관 필드(m.team)

* 일대다 조인 : 컬렉션 값 연관 필드(m.members)

```java
SELECT t, m

FROM Team t LEFT JOIN t.members m

```


#### 세타 조인

---

- WHERE 절을 사용한 세타 조인 (내부 조인만 지원)

- 전혀 관계없는 엔티티도 조인 가능

```java
SELECT count(m) 

FROM Member m, Team t

WHERE m.username = t.name
```

#### JOIN ON

---
- 조인 대상 필터링 (내부조인의 ON 절은 WHERE 절 대체 가능 -> 외부 조인에서만 사용)

### fetch Join

- JPQL에서 _성능 최적화_를 위해 제공하는 기능

- 연관된 엔티티나 컬렉션을 _한 번에 같이 조회_ (JPQL은 결과를 반환할 때 연관관계까지 고려하지 않음 -> fetch join)

  ㄴ SQL 호출 횟수를 줄여 성능 최적화

  ㄴ 쿼리 시점에 조회하므로 지연 로딩이 발생하지 않음

  ㄴ 준영속 상태에서도 객체 그래프를 탐색

- 글로벌 로딩 전략보다 우선

  

> fetch join의 한계

- 페치 조인 대상에는 별칭을 줄 수 없음

- 둘 이상의 컬렉션을 페치할 수 없음 (카타시안 곱 발생)

- 컬렉션을 페치 조인하면 페이징 API를 사용할 수 없음 (단일 값 연관 필드(일대일, 다대일)는 가능)

  

#### 엔티티 fetch join

---

- join fetch 라고 기입하면 연관된 엔티티나 컬렉션을 함께 조회

- fetch join 은 별칭을 사용할 수 없음

* 아래 결과는 회원 엔티티만 선택했는데 회원과 연관된 팀도 함께 조회


- SQL Query

```java
select m

from Member m join fetch m.team
```


```java
String jpql = "select m from Member m join fetch m.team"

List<Member> members = em.createQuery(jpql, Member.class)

                         .getResultList();

for (Member member : members ) {

    // 페치 조인으로 회원과 팀을 함께 조회해서 자연 로딩 발생 안 함

    System.out.println(member.getUsername());

    System.out.println(member.getTeam().name());

}
```




#### 컬렉션 페치 조인

---

- 일대다 컬렉션 패치 조인

* 아래 결과는 TEAM 테이블에서 '팀A'는 하나지만 MEMBER 테이블과 조인하면서 결과가 증가

  - 데이터가 아닌 객체 관점이므로 A팀에 2명의 회원이 있다면, 총 4개의 데이터가 조회되는 현상이 발생

    (distinct 추가)

```java
String jpql = "select distinct t

               from TEAM t join fetch t.members

               where t.name = '팀A'"

List<Team> teams = em.createQuery(jpql, Team.class)

                     .getResultList();

for (Team team : teams) {

    System.out.println(team.getName() + ", " + team);

    for (Member member : team.getMambers()) {

        // fetch join으로 팀과 회원을 함께 조회해서 지연 로딩 발생 안 함

        System.out.println(member.getUsername() + ", " + member);

    }

}
```



### 경로 표현식

---

* 상태 필드 (state field) : 단순히 값을 저장하기 위한 필드(필드 or 프로펄티)

* 연관 필드 (association field) : 연관관계를 위한 필드, 임베디드 타입 포함(필드 or 프로펄티)

- 단일 값 연관 필드 : @ManyToOne, @OneToOne, 대상이 엔티티

- 컬렉션 값 연관 필드 : @OneToMany, @ManyToMany, 대상이 컬렉션

  

* 경로 탐색을 사용한 묵시적 조인 시

- 항상 내부 조인

- 컬렉션에서 경로 탐색을 할 경우 명시적으로 조인해서 별칭을 얻어야 함

```java
@Entity

public class Member {

    @Id

    @GeneratedValue

    private Long id;

    @Column(name = "name")

    private String username;    // 상태 필드

    private Integer age;        // 상태 필드

    @ManyToOne(..)

    private Team team;            // 연관 필드 (단일 값 연관 필드)

    @OneToMany(..)

    private List<Order> orders;    // 연관 필드 (컬렉션 값 연관 필드)

}

```


**상태 필드 경로** : 경로 탐색의 끝 (더 이상 탐색 불가)

```java
select m.username, m.age

from Member m

```

**단일 값 연관 경로** : 묵시적으로 내부 조인 발생 (계속 탐색 가능)
```java
select o.member

from Order o
```

 **컬렉션 값 연관 경로** : 묵시적으로 내부 조인 발생 (더 이상 탐색 불가)

  단, _경로 탐색을 할 경우 FROM절에서 조인을 통해 별칭 획득 필요_

   컬렉션 크기를 구할 수 있는 size 기능 존재 (select t.members.size from Team t)

```java
select t.member

from Team t

-- 컬렉션에서 경로 탐색을 할 경우

select m.username

from Team t

join t.members m
```

### 서브 쿼리

---
* SQL과 다르게 WHERE, HAVING 절에서만 사용 가능
#### 서브 쿼리 함수

---
* [NOT] EXISTS (subquery)

```java
-- 팀 A소속 회원

select m

from Member m

where exists (select t

              from m.team t

              where t.name = '팀A')

```
[All | ANY | SOME] (subquery)
```java
-- 전체 상품 각각의 재고보다 주문량이 많은 주문들

select o

from Order o

where o.orderAmount > ALL (select p.stockAmount

                           from Product p)

```
[NOT] IN (subquery)
```java
-- 20세 이상을 보유한 팀

select t

from Team t

where t IN (select t2

            from Team t2 JOIN t2.members m2

            where m2.age >= 20)
```

### 조건식

---

  

**ㅇ 타입 표현**

[] 문자 : 'hello'

[] 숫자 : 10L (Long)

    10D (Double)

   10F (Float)

[] 날짜 : {d '2020-12-22'} (Date)

   {t '13-39-55'} (Time)

{ts '2020-12-22 13-39-55.123'} (DateTime)

[] Boolean : true, false

[] Enum : test.MemberType.Enum (패키지 명을 포함한 전체 이름)

[] 엔티티 타입 : TYPE(m) = Member

  

**ㅇ 연산자 우선 순위**

**ㅇ 논리 연산과 비교식**

**ㅇ Between, IN, Like, NULL**

- Between

```java
-- 나이가 10~20인 회원

select m

from Member m

where m.age between 10 and 20

```

- [NOT] IN
```java
-- 이름이 회원1이나 회원2인 회원

select m

from Member m

where m.username in ('회원1', '회원2')

```

- Like

- IS [NOT] NULL

**컬렉션 식**

- 빈 컬렉션 비교 : IS [NOT] EMPTY

```java
-- 주문이 하나라도 있는 회원

select m

from Member m

where m.orders if not empty

```


**스칼라 식**

- 문자 함수 : CONCAT, SUBSTRING, TRIM, LOWER, UPPER, LENGTH, LOCATE

- 수학 함수 : ABS, SQRT, MOD, SIZE, INDEX

- 날짜 함수 : CURRENT_DATE(현재 날짜), CURRENT_TIME(현재 시간), CURRENT_TIMESTAMP(현재 날짜 시간)

  

**CASE 식**

- CASE, COALESCE, NULLIF

### 다형성 쿼리

---
**ㅇ TYPE**

- 엔티티의 상속 구조에서 조회 대상을 특정 자식 타입으로 한정할 때 주로 사용

```java
-- Item 중 Book, Movie를 조회

select i

from Item i

where type(i) IN (Book, Movie)

```


```java
select i

from Item i

where treat(i as Book).author = 'kim'

```

### 엔티티 직접 사용

---
- 기본 키 값
- JPQL에서 엔티티 객체(Member)를 직접 사용하면 SQL에서는 해당 엔티티의 기본 키 값(Member.id)을 사용

### Named 쿼리: 정적 쿼리

---
- 동적 쿼리
	- JPQL을 문자로 완성해서 직접 넘기는 것

- 정적(Named) 쿼리
	- 미리 정의한 쿼리에 이름을 부여해서 필요할 때 사용
	- 한 번 정의하면 변경할 수 없는 정적인 쿼리



#### Named 쿼리

---
- 애플리케이션 로딩 시점에 JPQL 문법을 체크하고 미리 파싱 -> 오류를 빨리 발견

- 사용 시점에 파싱된 결과를 재사용 -> 성능상 이점

- 정적 SQL 생성 -> DB 죄회 성능 최적화 

- @NamedQuery Annotation으로 자바 코드에 작성하거나 XML 문서에 작성할 수 있음

  

##### @NamedQuery Annotation

1. @NamedQuery.name에 쿼리 이름 부여

   @NamedQuery.query에 사용할 쿼리 입력

```java
@Entity

@NamedQuery (

    name = "Member.findByUsername",

    query="select m from Member m where m.username = :username")

public class Member {

    // ...

}

```

+. 하나의 엔티티에 2개 이상의 Named Query를 정의할 경우 @NamedQueries

```java
@Entity

@NamedQueries ({

    @NamedQuery (

        name = "Member.findByUsername",

        query="select m from Member m where m.username = :username"),

    @NamedQuery (

        name = "Member.count",

        query="select count(m) from Member m")

})

public class Member {

    // ...

}
```

2. em.createNamedQuery() 메소드에 쿼리 이름을 입력하여 쿼리 사용

```java
List<Member> resultList = em.createNamedQuery("Member.findByUsername", Mamber.class)

                            .setParameter("username", "회원1")

                            .getResultList();

```


##### Named 쿼리를 XML에 정의

  

Named 쿼리 작성 시 XML을 사용하는 것이 편리

  

1. XML 작성

```xml
<?xml version="1.0" encoding="UTF-8"?>

<entity-mappings

        xmlns="http://java.sun.com/xml/ns/persistence/orm"

        version="2.1">

    <named-query name="Member.findByUsername">

        <query><![CDATA[

             select m

             from Member m

             where m.username = :username

        ]]></query>

    </named-query>

    <named-query name="Member.count">

        <query>

            select count(m)

            from Member m

        </query>

    </named-query>

</entity-mappings>

```

2. 정의한 xml 파일을 인식할 수 있도록 persistence.xml 에 아래 코드 추가

- XML이 Annotation보다 우선권

```xml
<?xml version="1.0" encoding="UTF-8"?>

<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">

    <persistence-unit name="test">

        <mapping-file> META-INF/Member.xml </mapping-file>

    </persistence-unit>

</persistence>

```

### JPQL 

Repository.java

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


Entity.java
```java
package org.example.entity;  
  
import lombok.Data;  
  
import javax.persistence.Entity;  
import javax.persistence.Id;  
import javax.persistence.Table;  
  
@Data  
@Entity    //  
@Table(name="op")   // 테이블 명과 클래스 명이 다른 경우 사용  
public class OpenApiJpaEntity {  
  
    private String SIGUN_NM; // 시군명  
//    private String SIGUN_CD; // 시군코드  
    private int ACDNT_YY; // 사고년도  
    private String ACDNT_DIV_NM; // 사고유형 구분  
    @Id //id를 무조건 지정  
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







## Querydsl


> **Q-Type**

 Q-Type 인스턴스를 사용하는 방법은 다음 2가지 방법이 있다.

```java
QMember qMember = new QMember("m"); //별칭 직접 지정
QMember qMember = QMember.member; //기본 인스턴스 사용
```

 직접 별칭을 넣어서 사용해도 되지만 보통 기본 인스턴스에서 생성된 별칭을 사용하는 것을 권장한다. (별칭을 직접 지정하는 방법은 같은 테이블을 조인하는 경우에 쓰인다)

```java
@Generated("com.querydsl.codegen.DefaultEntitySerializer")
public class QMember extends EntityPathBase<Member> {

    ...

    public static final QMember member = new QMember("member1"); // 별칭 member가 자동으로 생성된다.

    ...

}
```

위에서 작성한 테스트 코드를 static import를 이용해 기본 인스턴스 별칭을 사용하면 다음과 같다.

```java
@Test
public void search() {
    JPAQueryFactory queryFactory = new JPAQueryFactory(em);
    Member result = queryFactory
            .selectFrom(member)
            .where(member.username.eq("member1")
            .fetchOne();
    assertThat(findMember.getUsername()).isEqualTo("member1");
}
```

Querydsl으로 실행되는 JPQL은 다음 설정을 추가하여 볼 수 있다.

```java
spring.jpa.properties.hibernate.use_sql_comments: true
```

```java
    /* select
        m 
    from
        Member m 
    where
        m.username = ?1 */ select
            member0_.member_id as member_id1_1_,
            member0_.age as age2_1_,
            member0_.team_id as team_id4_1_,
            member0_.username as username3_1_ 
        from
            member member0_ 
        where
            member0_.username=?
```

---

> **검색 조건 쿼리**

- JPQL이 제공하는 검색 조건을 거의 대부분 제공한다.
- SimpleExpression에서 제공하는 검색 조건 메서드 확인 가능

**검색 조건 예시**

```java
member.username.eq("member1")		// username = 'member1'
member.username.ne("member1")		//username != 'member1'
member.username.eq("member1").not()	// username != 'member1'

member.username.isNotNull()	//이름이 is not null
member.age.in(10, 20)		// age in (10,20)
member.age.notIn(10, 20)	// age not in (10, 20)
member.age.between(10,30)	//between 10, 30

member.age.goe(30)	// age >= 30
member.age.gt(30)	// age > 30
member.age.loe(30)	// age <= 30
member.age.lt(30)	// age < 30

member.username.like("member%")		//like 검색
member.username.contains("member")	// like ‘%member%’ 검색
member.username.startsWith("member")	//like ‘member%’ 검색
...
```

WHERE 문에서 AND 조건을 파라미터로 처리가 가능하다

- where()에 파라미터로 검색조건을 추가하면 AND 조건으로 추가된다.
- **파라미터로 처리 시 null 값이 들어오면 무시한다**  
    (메서드 추출을 활용하여 동적 쿼리를 깔끔하게 처리할 수 있다.)

```java
@Test
public void searchAndParam() {
    List<Member> result1 = queryFactory
            .selectFrom(member)
//          .where(member.username.eq("member1")
//              .and(member.age.eq(10)))
            .where(member.username.eq("member1"),
                    member.age.eq(10))
            .fetch();
    assertThat(result1.size()).isEqualTo(1);
}
```

---

> **결과 조회**

- fetch() : 리스트 조회, 데이터 없으면 빈 리스트 반환(null 아니다)
- fetchOne() : 단 건 조회  
                    결과가 없으면 null  
                    결과가 둘 이상이면 NonUniqueResultException 예외 발생
- fetchFirst() : limit(1).fetchOne() 과 같다.
- fetchResults() : 페이징 정보 포함, total count 쿼리 추가 실행 **(Deprecated, 향후 미지원)**
- fetchCount() :  count 쿼리로 변경하여 count 수 조회 **(Deprecated, 향후 미지원)**

**결과 조회 예시**

```java
//List
List<Member> fetch = queryFactory
        .selectFrom(member)
        .fetch();
 
//단 건
Member findMember1 = queryFactory
        .selectFrom(member)
        .fetchOne();
        
//처음 한 건 조회
Member findMember2 = queryFactory
        .selectFrom(member)
        .fetchFirst();
        
//페이징에서 사용(Deprecated)
QueryResults<Member> results = queryFactory
        .selectFrom(member)
        .fetchResults();
        
//count 쿼리로 변경(Deprecated)
long count = queryFactory
        .selectFrom(member)
        .fetchCount();
```

---

> **정렬**

- desc() : 내림차순
- asc() : 올림차순
- nullsLast() : null 데이터 순서 부여(마지막)
- nullsFirst() : null 데이터 순서 부여(처음)

**정렬 예시**

```java
@Test
public void sort() {
    em.persist(new Member(null, 100));
    em.persist(new Member("member5", 100));
    em.persist(new Member("member6", 100));
    
    List<Member> result = queryFactory
            .selectFrom(member)
            .where(member.age.eq(100))
            .orderBy(member.age.desc(), member.username.asc().nullsLast())
            .fetch();
            
    for (Member member : result) {
        System.out.println("member = " + member);
    }
}
```

```java
member = Member(id=8, username=member5, age=100)
member = Member(id=9, username=member6, age=100)
member = Member(id=7, username=null, age=100)
```

---

> **페이징**

- offset(long offset) : 쿼리 결과에 대한 오프셋을 정의, 0부터 시작
- limit(long limit) : 쿼리 결과에 대한 제한/최대 결과를 정의

**1. 페이징 - 조회 건수 제한**

```java
@Test
public void paging1() {
    List<Member> result = queryFactory
            .selectFrom(member)
            .orderBy(member.username.desc())
            .offset(0)
            .limit(2)
            .fetch();

    for (Member member : result) {
        System.out.println("member = " + member);
    }
    assertThat(result.size()).isEqualTo(2);
}
```

```java
member = Member(id=6, username=member4, age=40)
member = Member(id=5, username=member3, age=30)
```

**2. 페이징 - count**

```java
@Test
public void paging2() {
    Long result = queryFactory
            .select(member.count())
            .from(member)
            .fetchOne();
    System.out.println("result = " + result);
}
```

```java
result = 4
```

 실무에서 페이징 쿼리를 작성할 때, 데이터를 조회하는 쿼리는 여러 테이블을 조인해야 하지만, count 쿼리는 조인이 필요 없는 경우가 많다. 그런데 자동화된 count 쿼리를 사용하는 fetchResults()는 모두 조인을 하여 count 쿼리를 해버리기 때문에 성능이 안 나온다. 결국 fetchResults()는 Deprecated가 됐으며, 따라서 count 전용 쿼리를 따로 작성하는 것이 좋다.

---

> **집합**

**1. 집합 함수**

JPQL이 제공하는 모든 집합 함수를 제공한다.

- count() : 카운트
- sum()   : 합
- avg()    : 평균
- max()   : 최대
- min()    : 최소

```java
@Test
@DisplayName("집합 함수")
public void aggregation() {
    List<Tuple> result = queryFactory
            .select(member.count(),
                    member.age.sum(),
                    member.age.avg(),
                    member.age.max(),
                    member.age.min())
            .from(member)
            .fetch();

    for (Tuple tuple : result) {
        System.out.println("tuple = " + tuple);
        assertThat(tuple.get(member.count())).isEqualTo(4);
        assertThat(tuple.get(member.age.sum())).isEqualTo(100);
        assertThat(tuple.get(member.age.avg())).isEqualTo(25);
        assertThat(tuple.get(member.age.max())).isEqualTo(40);
        assertThat(tuple.get(member.age.min())).isEqualTo(10);
    }
}
```

**2. 그룹화/집계**

- groupby() : 그룹화/집계
- having() : 그룹화/집계용 필터

```java
@Test
@DisplayName("그룹화/집계")
public void group() {
    List<Tuple> result = queryFactory
            .select(team.name, member.age.avg())
            .from(member)
            .join(member.team, team)
            .groupBy(team.name)
            .fetch();

    for (Tuple tuple : result) {
        System.out.println("tuple = " + tuple);
    }

    Tuple teamA = result.get(0);
    Tuple teamB = result.get(1);

    assertThat(teamA.get(team.name)).isEqualTo("teamA");
    assertThat(teamA.get(member.age.avg())).isEqualTo(15);
    assertThat(teamB.get(team.name)).isEqualTo("teamB");
    assertThat(teamB.get(member.age.avg())).isEqualTo(35);
}
```

```java
tuple = [teamA, 15.0]
tuple = [teamB, 35.0]
```

 마찬가지로 having 또한 사용할 수 있다.

---

> **조인 - 기본 조인**

- join(), innerJoin()  : 내부 조인(inner join)
- leftJoin()            : left 외부 조인(left outer join)
- rightJOin()          : right 외부 조인(right outer join)
- JPQL의 on과 성능 최적화를 위한 fetch 조인 제공
- 기본 문법 : join(조인 대상, 별칭으로 사용할 Q 타입)

**1. 기본 조인 join - inner join**

```java
@Test
public void join() throws Exception {
    QMember member = QMember.member;
    QTeam team = QTeam.team;
    List<Member> result = queryFactory
            .selectFrom(member)
            .join(member.team, team)
            .where(team.name.eq("teamA"))
            .fetch();
    
    assertThat(result).extracting("username").containsExactly("member1", "member2");
}
```

```java
/* select
        member1 
    from
        Member member1   
    inner join
        member1.team as team 
    where
        team.name = ?1 */ select
            member0_.member_id as member_id1_1_,
            member0_.age as age2_1_,
            member0_.team_id as team_id4_1_,
            member0_.username as username3_1_ 
        from
            member member0_ 
        inner join
            team team1_ 
                on member0_.team_id=team1_.team_id 
        where
            team1_.name=?
```

---

> **세타 조인**

- 연관관계가 없는 필드로 조인
- cross join
- 잘 사용하지 않으므로 참고만 하자.

**세타 조인 예시**

```java
@Test
public void theta_join() throws Exception {
    em.persist(new Member("teamA"));
    em.persist(new Member("teamB"));
    List<Member> result = queryFactory
            .select(member)
            .from(member, team)
            .where(member.username.eq(team.name))
            .fetch();
            
    assertThat(result).extracting("username").containsExactly("teamA", "teamB");
}
```

```java
    /* select
        member1 
    from
        Member member1,
        Team team 
    where
        member1.username = team.name */ select
            member0_.member_id as member_id1_1_,
            member0_.age as age2_1_,
            member0_.team_id as team_id4_1_,
            member0_.username as username3_1_ 
        from
            member member0_ cross 
        join
            team team1_ 
        where
            member0_.username=team1_.name
```

```java
m = Member(id=7, username=teamA, age=0)
m = Member(id=8, username=teamB, age=0)
```

---

> **조인 - on절**

**1. 조인 대상 필터링**

```java
/**
 * 예) 회원과 팀을 조인하면서, 팀 이름이 teamA인 팀만 조인, 회원은 모두 조회
 * JPQL: SELECT m, t FROM Member m LEFT JOIN m.team t on t.name = 'teamA'
 * SQL: SELECT m.*, t.* FROM Member m LEFT JOIN Team t ON m.TEAM_ID=t.id and t.name='teamA'
 */
@Test
public void join_on_filtering() throws Exception {
    List<Tuple> result = queryFactory
            .select(member, team)
            .from(member)
            .leftJoin(member.team, team).on(team.name.eq("teamA"))
            .fetch();
            
    for (Tuple tuple : result) {
        System.out.println("tuple = " + tuple);
    }
}
```

```java
    /* select
        member1,
        team 
    from
        Member member1   
    left join
        member1.team as team with team.name = ?1 */ select
            member0_.member_id as member_id1_1_0_,
            team1_.team_id as team_id1_2_1_,
            member0_.age as age2_1_0_,
            member0_.team_id as team_id4_1_0_,
            member0_.username as username3_1_0_,
            team1_.name as name2_2_1_ 
        from
            member member0_ 
        left outer join
            team team1_ 
                on member0_.team_id=team1_.team_id 
                and (
                    team1_.name=?
                )
```

```java
tuple=[Member(id=3, username=member1, age=10), Team(id=1, name=teamA)]
tuple=[Member(id=4, username=member2, age=20), Team(id=1, name=teamA)]
tuple=[Member(id=5, username=member3, age=30), null]
tuple=[Member(id=6, username=member4, age=40), null]
```

 on 절을 활용해 조인 대상을 필터링할 때, 외부 조인이 아니라 내부 조인(inner join)을 사용하면, where 절에서 필터링하는 것과 기능이 동일하다. 따라서 on 절을 활용한 조인 대상 필터링을 사용할 때, 내부 조인이면 익숙한 where 절로 해결하고, 정말 외부 조인이 필요한 경우에만 이 기능을 사용하자.

**2. 연관관계가 없는 엔티티 외부 조인**

```java
/**
 * 2. 연관관계 없는 엔티티 외부 조인
 * 예) 회원의 이름과 팀의 이름이 같은 대상 외부 조인
 * JPQL: SELECT m, t FROM Member m LEFT JOIN Team t on m.username = t.name
 * SQL: SELECT m.*, t.* FROM Member m LEFT JOIN Team t ON m.username = t.name
 */
@Test
public void join_on_no_relation() throws Exception {
    em.persist(new Member("teamA"));
    em.persist(new Member("teamB"));
    List<Tuple> result = queryFactory
            .select(member, team)
            .from(member)
            .leftJoin(team).on(member.username.eq(team.name))
            .fetch();
            
    for (Tuple tuple : result) {
        System.out.println("t=" + tuple);
    }
}
```

```java
    /* select
        member1,
        team 
    from
        Member member1   
    left join
        Team team with member1.username = team.name */ select
            member0_.member_id as member_id1_1_0_,
            team1_.team_id as team_id1_2_1_,
            member0_.age as age2_1_0_,
            member0_.team_id as team_id4_1_0_,
            member0_.username as username3_1_0_,
            team1_.name as name2_2_1_ 
        from
            member member0_ 
        left outer join
            team team1_ 
                on (
                    member0_.username=team1_.name
                )
```

```java
t=[Member(id=5, username=member3, age=30), null]
t=[Member(id=6, username=member4, age=40), null]
t=[Member(id=3, username=member1, age=10), null]
t=[Member(id=4, username=member2, age=20), null]
```

 하이버네이트 5.1부터 ON을 사용하여 서로 관계가 없는 필드를 외부 조인하는 기능이 추가되었다. 주의할 점은 leftJoin()의 전달 인자가 달라진다.

- 일반 조인 : leftJoin(member.team, tema)
- on조인 : from(member).leftJoin(team).on(xxx)

---

> **조인 - 페치 조인**

- SQL조인을 활용해서 연관된 엔티티를 SQL 한 번에 조회하는 기능
- 주로 성능 최적화에 사용한다.
- join(), leftJoin() 등 조인 기능 뒤에 fetchJoin()이라고 추가하면 된다.

```java
@PersistenceUnit
EntityManagerFactory emf;

@Test
public void fetchJoinUse() throws Exception {
    em.flush();
    em.clear();
    Member findMember = queryFactory
            .selectFrom(member)
            .join(member.team, team).fetchJoin()
            .where(member.username.eq("member1"))
            .fetchOne();
    boolean loaded = emf.getPersistenceUnitUtil().isLoaded(findMember.getTeam());
    assertThat(loaded).as("페치 조인 적용").isTrue();
}
```

```java
    /* select
        member1 
    from
        Member member1   
    inner join
        fetch member1.team as team 
    where
        member1.username = ?1 */ select
            member0_.member_id as member_id1_1_0_,
            team1_.team_id as team_id1_2_1_,
            member0_.age as age2_1_0_,
            member0_.team_id as team_id4_1_0_,
            member0_.username as username3_1_0_,
            team1_.name as name2_2_1_ 
        from
            member member0_ 
        inner join
            team team1_ 
                on member0_.team_id=team1_.team_id 
        where
            member0_.username=?
```

---

> **서브 쿼리**

- com.querydsl.jpa.JPAExpressions 사용

**1. 서브 쿼리**

```java
/**
 * 나이가 가장 많은 회원 조회
 */
@Test
public void subQuery() throws Exception {

    QMember memberSub = new QMember("memberSub");

    List<Member> result = queryFactory
            .selectFrom(member)
            .where(member.age.eq(
                JPAExpressions
                    .select(memberSub.age.max())
                    .from(memberSub)
            ))
            .fetch();
    assertThat(result).extracting("age").containsExactly(40);
}
```

```java
    /* select
        member1 
    from
        Member member1 
    where
        member1.age = (
            select
                max(memberSub.age) 
            from
                Member memberSub
        ) */ select
            member0_.member_id as member_id1_1_,
            member0_.age as age2_1_,
            member0_.team_id as team_id4_1_,
            member0_.username as username3_1_ 
        from
            member member0_ 
        where
            member0_.age=(
                select
                    max(member1_.age) 
                from
                    member member1_
            )
```

**2. 서브 쿼리 - in 사용**

```java
/**
 * 서브쿼리 여러 건 처리, in 사용
 */
@Test
public void subQueryIn() throws Exception {
    QMember memberSub = new QMember("memberSub");
    List<Member> result = queryFactory
            .selectFrom(member)
            .where(member.age.in(
                JPAExpressions
                    .select(memberSub.age)
                    .from(memberSub)
                    .where(memberSub.age.gt(10))
            ))
        	.fetch();
    assertThat(result).extracting("age").containsExactly(20, 30, 40);
}
```

```java
    /* select
        member1 
    from
        Member member1 
    where
        member1.age in (
            select
                memberSub.age 
            from
                Member memberSub 
            where
                memberSub.age > ?1
        ) */ select
            member0_.member_id as member_id1_1_,
            member0_.age as age2_1_,
            member0_.team_id as team_id4_1_,
            member0_.username as username3_1_ 
        from
            member member0_ 
        where
            member0_.age in (
                select
                    member1_.age 
                from
                    member member1_ 
                where
                    member1_.age>?
            )
```

**3. 서브 쿼리 - select 절에서 사용**

```java
@Test
@DisplayName("서브 쿼리 select 절에서 사용")
public void subQuerySelect() {
    QMember memberSub = new QMember("memberSub");
    List<Tuple> fetch = queryFactory
            .select(member.username,
                    JPAExpressions
                            .select(memberSub.age.avg())
                            .from(memberSub)
            ).from(member)
            .fetch();

    for (Tuple tuple : fetch) {
        System.out.println("username = " + tuple.get(member.username));
        System.out.println("age = " +
                tuple.get(JPAExpressions.select(memberSub.age.avg())
                        .from(memberSub)));
    }
}
```

```java
    /* select
        member1.username,
        (select
            avg(memberSub.age) 
        from
            Member memberSub) 
    from
        Member member1 */ select
            member0_.username as col_0_0_,
            (select
                avg(member1_.age) 
            from
                member member1_) as col_1_0_ 
        from
            member member0_
```

```java
username = member1
age = 25.0
username = member2
age = 25.0
username = member3
age = 25.0
username = member4
age = 25.0
```

**3. static import 사용**

```java
import static com.querydsl.jpa.JPAExpressions.select;


List<Member> result = queryFactory
        .selectFrom(member)
        .where(member.age.eq(
                select(memberSub.age.max())
                    .from(memberSub)	
        ))
        .fetch();
```

**form 절의 서브쿼리 한계**

 JPA JPQL 서브쿼리의 한계로 **from 절의 서브쿼리(인라인 뷰)는 지원하지 않는다.** 당연히 Querydsl 도 지원하지 않는다. JPA에서는 select 절의 서브쿼리도 지원하지 않지만 하이버네이트 구현체를 사용하면 select 절의 서브쿼리는 지원하기 때문에 Querydsl도 하이버네이트 구현체를 사용하여 select 절의 서브쿼리를 지원한다.

**from 절의 서브쿼리 해결방안**

- 서브쿼리를 join으로 변경한다. (가능한 상황도 있고, 불가능한 상황도 있다.)
- 애플리케이션에서 쿼리를 2번 분리해서 실행한다.
- nativeSQL을 사용한다.

---

> **Case 문**

**1. 단순한 조건**

```java
@Test
@DisplayName("case 테스트")
public void basicCase() {
    List<String> result = queryFactory
            .select(member.age
                    .when(10).then("열살")
                    .when(20).then("스물살")
                    .otherwise("기타"))
            .from(member)
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

```java
    /* select
        case 
            when member1.age = ?1 then ?2 
            when member1.age = ?3 then ?4 
            else '기타' 
        end 
    from
        Member member1 */ select
            case 
                when member0_.age=? then ? 
                when member0_.age=? then ? 
                else '기타' 
            end as col_0_0_ 
        from
            member member0_
```

```java
s = 열살
s = 스물살
s = 기타
s = 기타
```

**2. 복잡한 조건**

```java
@Test
@DisplayName("case 테스트 2")
public void complexCase() {
    List<String> result = queryFactory
            .select(new CaseBuilder()
                    .when(member.age.between(0, 20)).then("0~20살")
                    .when(member.age.between(21, 30)).then("21~30살")
                    .otherwise("기타"))
            .from(member)
            .fetch();

    for (String s : result) {
        System.out.println("s = " + s);
    }
}
```

```java
    /* select
        case 
            when (member1.age between ?1 and ?2) then ?3 
            when (member1.age between ?4 and ?5) then ?6 
            else '기타' 
        end 
    from
        Member member1 */ select
            case 
                when member0_.age between ? and ? then ? 
                when member0_.age between ? and ? then ? 
                else '기타' 
            end as col_0_0_ 
        from
            member member0_
```

```java
s = 0~20살
s = 0~20살
s = 21~30살
s = 기타
```

**3. orderBy에서 Case 문 함께 사용**

```java
@Test
@DisplayName("orderBy에서 case 문 함께 사용")
public void orderByWithCase() {

    NumberExpression<Integer> rankPath = new CaseBuilder()
            .when(member.age.between(0, 20)).then(2)
            .when(member.age.between(21, 30)).then(1)
            .otherwise(3);

    List<Tuple> result = queryFactory
            .select(member.username, member.age, rankPath)
            .from(member)
            .orderBy(rankPath.desc())
            .fetch();

    for (Tuple tuple : result) {
        String username = tuple.get(member.username);
        Integer age = tuple.get(member.age);
        Integer rank = tuple.get(rankPath);
        System.out.println("username = " + username + " age = " + age + " rank = "
                + rank);
    }
}
```

```java
    /* select
        member1.username,
        member1.age,
        case 
            when (member1.age between ?1 and ?2) then ?3 
            when (member1.age between ?4 and ?5) then ?6 
            else 3 
        end 
    from
        Member member1 
    order by
        case 
            when (member1.age between ?7 and ?8) then ?9 
            when (member1.age between ?10 and ?11) then ?12 
            else 3 
        end desc */ select
            member0_.username as col_0_0_,
            member0_.age as col_1_0_,
            case 
                when member0_.age between ? and ? then ? 
                when member0_.age between ? and ? then ? 
                else 3 
            end as col_2_0_ 
        from
            member member0_ 
        order by
            case 
                when member0_.age between ? and ? then ? 
                when member0_.age between ? and ? then ? 
                else 3 
            end desc
```

```java
username = member4 age = 40 rank = 3
username = member1 age = 10 rank = 2
username = member2 age = 20 rank = 2
username = member3 age = 30 rank = 1
```

---

> **상수, 문자 더하기**

**1. 상수 더하기 - Expressions.constant(xxx)**

※ 참고 : 최적화를 위해 가능하면 SQL에 contstant 값을 넘기지 않고 해결하는 것이 좋다.

```java
Tuple result = queryFactory
        .select(member.username, Expressions.constant("A"))
        .from(member)
        .fetchFirst();
```

```java
    /* select
        member1.username 
    from
        Member member1 */ select
            member0_.username as col_0_0_ 
        from
            member member0_ fetch first ? rows only

// 결과
result = [member1, A]
```

**2. 문자 더하기 - concat**

```java
String result = queryFactory
        .select(member.username.concat("_").concat(member.age.stringValue()))
        .from(member)
        .where(member.username.eq("member1"))
        .fetchOne();
```

```java
    /* select
        concat(concat(member1.username,
        ?1),
        str(member1.age)) 
    from
        Member member1 
    where
        member1.username = ?2 */ select
            member0_.username||?||to_char(member0_.age) as col_0_0_ 
        from
            member member0_ 
        where
            member0_.username=?

// 결과
result = member1_10
```

 member.age.stringValue() 부분이 중요한데, 문자가 아닌 다른 타입들은 stringValue()로 문자로 변환할 수 있다. 이 방법은 ENUM을 처리할 때도 자주 사용한다




### 내가 만든 
---


pom.xml qClass 만드는 dependency 밑에 플러그인도 설치  무조건 qclass 생성성
```java
<!-- https://mvnrepository.com/artifact/com.querydsl/querydsl-jpa -->
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-jpa</artifactId>
            <version>5.0.0</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/com.querydsl/querydsl-core -->
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-core</artifactId>
            <version>5.0.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.querydsl/querydsl-apt -->
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-apt</artifactId>
            <version>5.0.0</version>
        </dependency>


<project>
  <build>
  <plugins>
    ...
    <plugin>
      <groupId>com.mysema.maven</groupId>
      <artifactId>apt-maven-plugin</artifactId>
      <version>1.1.3</version>
      <executions>
        <execution>
          <goals>
            <goal>process</goal>
          </goals>
          <configuration>
            <outputDirectory>target/generated-sources/java</outputDirectory>
            <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
          </configuration>
        </execution>
      </executions>
    </plugin>
    ...
  </plugins>
  </build>
</project>


```

라이브러리 생성후 오른쪽의 
![[JPA61.png]]


![[JPA62.png]]


compile 누르고 실행 버튼 클릭


### qClass 생성후

```java
public static QOpenApiJpaEntity openApiEntity; 써주기
```

ex)
```java
package org.example.entity;  
  
import static com.querydsl.core.types.PathMetadataFactory.*;  
  
import com.querydsl.core.types.dsl.*;  
  
import com.querydsl.core.types.PathMetadata;  
import javax.annotation.Generated;  
import com.querydsl.core.types.Path;  
  
  
/**  
 * QOpenApiJpaEntity is a Querydsl query type for OpenApiJpaEntity */@Generated("com.querydsl.codegen.DefaultEntitySerializer")  
public class QOpenApiJpaEntity extends EntityPathBase<OpenApiJpaEntity> {  
  
    private static final long serialVersionUID = -47515317L;  
  
    public static final QOpenApiJpaEntity openApiJpaEntity = new QOpenApiJpaEntity("openApiJpaEntity");  
    public static QOpenApiJpaEntity openApiEntity;   //여기에 중간에 쓰면 됨 사실 위치는 상관 x
  
    public final StringPath ACDNT_DIV_NM = createString("ACDNT_DIV_NM");  
  
    public final NumberPath<Integer> ACDNT_YY = createNumber("ACDNT_YY", Integer.class);  
  
    public final NumberPath<Integer> CASLT_CNT = createNumber("CASLT_CNT", Integer.class);  
  
    public final NumberPath<Integer> DPRS_CNT = createNumber("DPRS_CNT", Integer.class);  
  
    public final NumberPath<Integer> INJURY_APLCNT_CNT = createNumber("INJURY_APLCNT_CNT", Integer.class);  
  
    public final StringPath JURISD_POLCSTTN_NM = createString("JURISD_POLCSTTN_NM");  
  
    public final NumberPath<Double> LAT = createNumber("LAT", Double.class);  
  
    public final StringPath LEGALDONG_CD_NO = createString("LEGALDONG_CD_NO");  
  
    public final StringPath LOC_INFO = createString("LOC_INFO");  
  
    public final NumberPath<Double> LOGT = createNumber("LOGT", Double.class);  
  
    public final StringPath MULTI_KNOWLG_DIV_GROUP_NO = createString("MULTI_KNOWLG_DIV_GROUP_NO");  
  
    public final StringPath MULTI_KNOWLG_DIV_NO = createString("MULTI_KNOWLG_DIV_NO");  
  
    public final StringPath MULTI_REGION_INFO = createString("MULTI_REGION_INFO");  
  
    public final NumberPath<Integer> OCCUR_CNT = createNumber("OCCUR_CNT", Integer.class);  
  
    public final NumberPath<Integer> SERINJRY_INDVDL_CNT = createNumber("SERINJRY_INDVDL_CNT", Integer.class);  
  
    public final StringPath SIGUN_NM = createString("SIGUN_NM");  
  
    public final NumberPath<Integer> SLTINJRY_INDVDL_CNT = createNumber("SLTINJRY_INDVDL_CNT", Integer.class);  
  
    public final StringPath SPOT_NO = createString("SPOT_NO");  
  
    public QOpenApiJpaEntity(String variable) {  
        super(OpenApiJpaEntity.class, forVariable(variable));  
    }  
  
    public QOpenApiJpaEntity(Path<? extends OpenApiJpaEntity> path) {  
        super(path.getType(), path.getMetadata());  
    }  
  
    public QOpenApiJpaEntity(PathMetadata metadata) {  
        super(OpenApiJpaEntity.class, metadata);  
    }  
  
}
```





Repository
```java
package org.example.repository;  
  
import com.querydsl.jpa.impl.JPAQueryFactory;  
import lombok.extern.slf4j.Slf4j;  
import org.example.entity.OpenApiJpaEntity;  
import org.example.entity.QOpenApiJpaEntity;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.data.jpa.repository.JpaRepository;  
import org.springframework.stereotype.Repository;  
import org.springframework.transaction.annotation.Transactional;  
  
import javax.persistence.EntityManager;  
import javax.persistence.PersistenceContext;  
import java.util.List;  
@Slf4j  
@Repository  
public class OpenApiJpaRepository{  
  
    @PersistenceContext  
    private EntityManager em;  
  
    public List<OpenApiJpaEntity> selectAll(){  
        return em.createQuery("select op from OpenApiJpaEntity op", OpenApiJpaEntity.class).getResultList();  
    }  
  
  //querydsl 방식식
    public void t(){  
        OpenApiJpaEntity openApiJpaEntity = new OpenApiJpaEntity();  
//        em.persist(openApiJpaEntity);  
        JPAQueryFactory jpaQueryFactory = new JPAQueryFactory(em);  
        QOpenApiJpaEntity qOpenApiJpaEntity = new QOpenApiJpaEntity("op");  
        List<Double> ar = jpaQueryFactory.select(qOpenApiJpaEntity.LAT).from(qOpenApiJpaEntity).fetch();  
        log.error(String.valueOf(ar.size()));  
        System.out.println(ar.size());  
//        System.out.println(jpaQueryFactory.selectDistinct(qOpenApiJpaEntity.LAT).from(qOpenApiJpaEntity).fetch());  
  
    }  
  
  
  
  
}
```


```java
QOpenApiJpaEntity op = QOpenApiJpaEntity.openApiJpaEntity;
List<OpenApiJpaEntity> resultList = new JPAQueryFactory(em)
        .select(op)  //select op는 table객체를 의미 *을 사용할수 없으므로 op사용
        .from(op) // from
        .fetch();  // 쿼리를 실행하고 결과를 가져옴


QOpenApiJpaEntity op = QOpenApiJpaEntity.openApiJpaEntity;
List<Tuple> result = new JPAQuery<>(entityManager)
    .select(op.LAT, op.LOGT)
    .from(qEntity)
    .fetch();
    
```


```java
.gt  // `gt`는 QueryDSL에서 제공하는 메서드 중 하나로, "greater than"을 의미합니다. 이 메서드는 주어진 값보다 큰 값을 비교하는 조건을 생성
QueryDSL은 다양한 조건을 표현하기 위한 다양한 메서드를 제공하며, `gt` 외에도 `eq` (equal), `lt` (less than), `gte` (greater than or equal to), `lte` (less than or equal to) 등의 메서드를 사용할 수 있습니다. 이를 조합하여 복잡한 쿼리를 작성할 수 있습니다.
```

```javascript
.and는  AND 연산을 의미

Predicate condition = user.name.eq(name)
            .and(user.age.gt(age)); 이 의미가 뭐야
``` 


Entity.java
```java
package org.example.entity;  
  
import lombok.Data;  
import lombok.Getter;  
import lombok.Setter;  
  
import javax.persistence.Entity;  
import javax.persistence.Id;  
import javax.persistence.Table;  
  
@Getter  
@Setter  
@Entity    //  
@Table(name="op")   // 테이블 명과 클래스 명이 다른 경우 사용  
public class OpenApiJpaEntity {  
  
    private String SIGUN_NM; // 시군명  
//    private String SIGUN_CD; // 시군코드  
    private int ACDNT_YY; // 사고년도  
    private String ACDNT_DIV_NM; // 사고유형 구분  
    @Id //id를 무조건 지정  
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




1. Querydsl Q 클래스 생성: Querydsl은 엔티티 클래스를 기반으로 Q 클래스를 생성해야 합니다. 이 Q 클래스는 엔티티의 속성을 정적 필드로 가지고 있어 쿼리 작성 시 사용됩니다.
    
2. Querydsl 쿼리 작성: Q 클래스를 사용하여 Querydsl 쿼리를 작성합니다. Querydsl은 fluent API를 제공하여 간결하고 가독성 있는 코드를 작성할 수 있습니다.
    
3. 쿼리 실행: 작성된 Querydsl 쿼리를 JPA의 `createQuery()` 메서드를 통해 실행할 수 있습니다. 실행 결과로는 `JPQLQuery` 객체가 반환됩니다.
    
4. 결과 처리: `JPQLQuery` 객체를 사용하여 결과를 가져올 수 있습니다. `fetch()` 메서드를 사용하면 결과를 리스트 형태로 반환받을 수 있고, `fetchOne()` 메서드를 사용하면 단일 결과를 반환받을 수 있습니다.












--- 
참조 - https://jddng.tistory.com/334

https://data-make.tistory.com/614
