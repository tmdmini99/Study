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










