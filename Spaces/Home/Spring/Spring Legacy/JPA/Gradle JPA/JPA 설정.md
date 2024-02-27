
```java
@Query("select p from Post p join fetch p.user u "
    + "where u in "
    + "(select t from Follow f inner join f.target t on f.source = :user) "
    + "or u = :user "
    + "order by p .createdAt desc")
List<Post> findAllAssociatedPostsByUser(@Param("user") User user, Pageable pageable);
```

Spring Data JPA가 기본적으로 제공해주는 CRUD 메서드 및 쿼리 메서드 기능을 사용하더라도, 원하는 조건의 데이터를 수집하기 위해서는 필연적으로 JPQL을 작성하게 됩니다. 간단한 로직을 작성하는데 큰 문제는 없으나, 복잡한 로직의 경우 개행이 포함된 쿼리 문자열이 상당히 길어집니다. JPQL 문자열에 오타 혹은 문법적인 오류가 존재하는 경우, 정적 쿼리라면 어플리케이션 로딩 시점에 이를 발견할 수 있으나 그 외는 런타임 시점에서 에러가 발생합니다.

이러한 문제를 어느 정도 해소하는데 기여하는 프레임워크가 바로 QueryDSL입니다. QueryDSL은 정적 타입을 이용해서 SQL 등의 쿼리를 생성해주는 프레임워크입니다. QueryDSL의 장점은 다음과 같습니다.

1. 문자가 아닌 코드로 쿼리를 작성함으로써, 컴파일 시점에 문법 오류를 쉽게 확인할 수 있다.
2. 자동 완성 등 IDE의 도움을 받을 수 있다.
3. 동적인 쿼리 작성이 편리하다.
4. 쿼리 작성 시 제약 조건 등을 메서드 추출을 통해 재사용할 수 있다.

## 2. 설정

사실 QueryDSL을 적용하면서 가장 까다로운 부분이 설정이었습니다. 공식 문서에는 Gradle에 대한 내용이 누락되어 있으며, 실제로 QueryDSL 설정 방법은 Gradle 및 IntelliJ 버전에 따라 상이하기 때문입니다. 따라서 제가 사용하는 방법이 다른 환경에서는 잘 동작하지 않을 수 있습니다. 😭 Groovy 문법이 익숙하지 않다면 설정 파일을 완벽하게 이해할 필요는 없어보입니다.

> build.gradle


```gradle
buildscript {
    ext {
        queryDslVersion = "4.4.0"
    }
}

plugins {
    id 'org.springframework.boot' version '2.5.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group = 'com.learning'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    runtimeOnly 'com.h2database:h2'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'

    // QueryDSL
    implementation "com.querydsl:querydsl-jpa:${queryDslVersion}"
    annotationProcessor(
            "javax.persistence:javax.persistence-api",
            "javax.annotation:javax.annotation-api",
            "com.querydsl:querydsl-apt:${queryDslVersion}:jpa")
}

// QueryDSL
sourceSets {
    main {
        java {
            srcDirs = ["$projectDir/src/main/java", "$projectDir/build/generated"]
        }
    }
}

test {
    useJUnitPlatform()
}
```

- 기본적으로 QueryDSL은 프로젝트 내의 @Entity 어노테이션을 선언한 클래스를 탐색하고, `JPAAnnotationProcessor`를 사용해 Q 클래스를 생성합니다.
    - Q 클래스는 후술합니다.
- `querydsl-apt`가 @Entity 및 @Id 등의 애너테이션을 알 수 있도록, `javax.persistence`과 `javax.annotation`을 annotationProcessor에 함께 추가합니다.
    - `annotationProcessor`는 Java 컴파일러 플러그인으로서, 컴파일 단계에서 어노테이션을 분석 및 처리함으로써 추가적인 파일을 생성할 수 있습니다.
- 개발 환경에서 생성된 Q 클래스를 사용할 수 있도록 generated 디렉토리를 sourceSet에 추가합니다.
    - IDE의 개발 코드에서 생성된 Q 클래스 파일에 접근할 수 있습니다.

### 2.1. Q 클래스



![[Pasted image 20240227152245.png]]



---
참조 - https://tecoble.techcourse.co.kr/post/2021-08-08-basic-querydsl/