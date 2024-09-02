
src
└── main
    ├── java
    │   └── com
    │       └── example
    │           └── mvcquerydsldemo
    │               ├── MvcQuerydslDemoApplication.java
    │               ├── controller
    │               │   └── UserController.java
    │               ├── model
    │               │   └── User.java
    │               ├── repository
    │               │   ├── UserRepository.java
    │               │   ├── UserRepositoryCustom.java
    │               │   └── UserRepositoryImpl.java
    │               └── service
    │                   └── UserService.java
    └── resources
        ├── application.properties
        └── data.sql



pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>mvc-querydsl-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>
    <name>mvc-querydsl-demo</name>
    <description>Demo project for Spring Boot with MVC and QueryDSL</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>17</java.version>
        <querydsl.version>5.0.0</querydsl.version>
    </properties>

    <dependencies>
        <!-- Spring Boot Starters -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- QueryDSL -->
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-apt</artifactId>
            <version>${querydsl.version}</version>
            <scope>provided</scope>
        </dependency>

        <!-- H2 Database -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
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
        </plugins>
    </build>
</project>

```



2. 엔터티 클래스 (`User.java`)

```java
package com.example.mvcquerydsldemo.model;

import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
@Getter
@Setter
@NoArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String username;
    private String email;
}

```


UserRepository.java

```java
package com.example.mvcquerydsldemo.repository;

import com.example.mvcquerydsldemo.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long>, UserRepositoryCustom {
}

```

UserRepositoryCustom.java

```java
package com.example.mvcquerydsldemo.repository;

import com.example.mvcquerydsldemo.model.User;

import java.util.List;

public interface UserRepositoryCustom {
    List<User> findUsersByUsername(String username);
}

```


UserRepositoryImpl.java

```java
package com.example.mvcquerydsldemo.repository;

import com.example.mvcquerydsldemo.model.QUser;
import com.example.mvcquerydsldemo.model.User;
import com.querydsl.jpa.impl.JPAQueryFactory;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import java.util.List;

@Repository
public class UserRepositoryImpl implements UserRepositoryCustom {

    @PersistenceContext
    private EntityManager em;

    @Override
    public List<User> findUsersByUsername(String username) {
        JPAQueryFactory queryFactory = new JPAQueryFactory(em);
        QUser user = QUser.user;

        return queryFactory.selectFrom(user)
                           .where(user.username.eq(username))
                           .fetch();
    }
}

```


UserService.java

```java
package com.example.mvcquerydsldemo.service;

import com.example.mvcquerydsldemo.model.User;
import com.example.mvcquerydsldemo.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public User saveUser(User user) {
        return userRepository.save(user);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public List<User> getUsersByUsername(String username) {
        return userRepository.findUsersByUsername(username);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}

```


UserController.java

```java
package com.example.mvcquerydsldemo.controller;

import com.example.mvcquerydsldemo.model.User;
import com.example.mvcquerydsldemo.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.saveUser(user);
    }

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{username}")
    public List<User> getUsersByUsername(@PathVariable String username) {
        return userService.getUsersByUsername(username);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}

```


application.properties

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true

```


초기 데이터 로딩 (`data.sql`)


```sql
INSERT INTO user (username, email) VALUES ('john_doe', 'john@example.com');
INSERT INTO user (username, email) VALUES ('jane_doe', 'jane@example.com');

```


- **Model**: `User` 엔터티는 데이터베이스 테이블과 매핑되며, 사용자 데이터를 나타냅니다.
- **View**: REST API를 통해 JSON 형식으로 데이터를 주고받습니다.
- **Controller**: `UserController`는 HTTP 요청을 처리하며, 사용자 데이터를 조회, 생성, 삭제하는 API를 제공합니다.
- **Service**: `UserService`는 비즈니스 로직을 처리하며, 리포지토리와 상호작용합니다.
- **Repository**: `UserRepository`와 `UserRepositoryImpl`은 데이터베이스에 대한 CRUD 작업을 수행합니다. QueryDSL을 사용하여 `findUsersByUsername` 메서드에서 동적 쿼리를 작성합니다.