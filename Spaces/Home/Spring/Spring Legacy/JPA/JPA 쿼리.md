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

### JPQL 간단하게 사용

```java

```



## Querydsl




pom.xml qClass 만드는 dependency 밑에 플러그인도 설치
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









1. Querydsl Q 클래스 생성: Querydsl은 엔티티 클래스를 기반으로 Q 클래스를 생성해야 합니다. 이 Q 클래스는 엔티티의 속성을 정적 필드로 가지고 있어 쿼리 작성 시 사용됩니다.
    
2. Querydsl 쿼리 작성: Q 클래스를 사용하여 Querydsl 쿼리를 작성합니다. Querydsl은 fluent API를 제공하여 간결하고 가독성 있는 코드를 작성할 수 있습니다.
    
3. 쿼리 실행: 작성된 Querydsl 쿼리를 JPA의 `createQuery()` 메서드를 통해 실행할 수 있습니다. 실행 결과로는 `JPQLQuery` 객체가 반환됩니다.
    
4. 결과 처리: `JPQLQuery` 객체를 사용하여 결과를 가져올 수 있습니다. `fetch()` 메서드를 사용하면 결과를 리스트 형태로 반환받을 수 있고, `fetchOne()` 메서드를 사용하면 단일 결과를 반환받을 수 있습니다.










