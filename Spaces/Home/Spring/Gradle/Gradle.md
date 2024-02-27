

# Gradle 이란?

gradle은 오픈소스 빌드 자동화 툴로, 거의 모든 타입의 소프트웨어를 빌드할 수 있는 유연함을 가집니다.

# Gradle 의 특징

## High Performance

Gradle은 실행시켜야 하는 task만 실행시키고 다른 불필요한 동작은 하지 않으며, build cache를 사용함으로써 이전 실행의 task output을 재사용합니다. 심지어 서로 다른 기계에서도 build cache를 공유하여 성능을 높힐 수 있습니다.

## JVM foundation

Gradle은 JVM 에서 실행되기 때문에 JDK를 설치해야 합니다. 또한 Java Standard API를 빌드 로직에 사용할 수 있으며 , 다양한 플랫폼에서 실행할 수 있습니다.

## Convetions

Gradle은 Maven으로부터 의존 라이브러리 관리 기능을 차용했습니다. 따라서 컨벤션을 따라 Java 프로젝트와 같은 일반적인 유형의 프로젝트를 쉽게 빌드할 수 있으며, 필요하다면 컨벤션을 오버라이딩하거나 task를 추가해 컨벤션 기반의 빌드를 커스터마이징할 수 있습니다.

## Extensibility

Gradle을 확장하면 고유의 task 타입을 제공하거나 모델을 빌드할 수 있습니다.

## IDE , Build Scan support

Android Studio , IntelliJ IDEA , Eclipse 등의 IDE에서 Gradle을 임포트하여 사용할 수 있으며 , 빌드를 모니터링할 수 있는 Build Scan을 지원합니다.

# Spring 프로젝트에서의 Gradle

## 파일 구성

Spring을 Gradle 프로젝트로 생성하면 다음과 같은 구조를 갖게됩니다.

> ├─ gradle  
> │ └─ wrapper  
> │ ├─ gradle-wrapper.jar  
> │ └─ gradle-wrapper.properties  
> ├─ gradlew  
> ├─ gradlew.bat  
> ├─ build.gradle  
> └─ settings.gradle

|파일명|역할|
|---|---|
|gradlew|리눅스 또는 맥OS용 실행 쉘 스크립트 파일|
|gradlew.bat|윈도우용 실행 배치 스크립트 파일|
|gradle-wrapper.jar|Jar 형식으로 압축된 Wrapper 파일로, gradlew , gradlew.bat 파일이 해당 jar 파일을 이용해 gradle task를 실행|
|gradle-wrapper.properties|gradle Wrapper 설정 정보 파일로 , Wrapper의 버전 등을 설정|
|build.gradle|프로젝트의 라이브러리 의존성 , 플러그인 , 라이브러리 저장소 등을 설정하는 빌드 스크립트 파일|
|settings.gradle|프로젝트의 구성 정보 파일로, 프로젝트를 모듈화 할때, 하위 프로젝트의 구성을 설정|

## 라이브러리 의존성 관리


![[Gradle1.png]]


의존성은 종종 모듈로 제공되는데, 이 모듈들을 저장하는 곳을 `repository`라고 합니다. repository는 로컬 저장소가 될 수 있고, 원격 저장소가 될 수 있습니다. Gradle에게 어디서 의존성을 가져올지 알려줘야 하는데 이는 repository 선언을 통해 할 수 있습니다.

Gradle은 특정 task 를 실행시키기 위해 필요한 의존성들을 런타임시에 원격 저장소에서 다운로드 받거나 로컬 저장소에서 가져옵니다. 혹은 멀티 프로젝트일 경우 다른 프로젝트에서 가져오는데 이 과정을 `dependency resolution` 이라고 합니다.

Gradle은 향후 불필요한 네트워크 호출을 하지 않기 위해 의존성 파일들을 `dependency cache`라고 하는 로컬 캐시에 저장합니다.

라이브러리 의존성 설정은 `build.gradle` 스크립트 파일에 작성할 수 있습니다. build.gradle 파일 내부의 코드를 보면 플러그인 , 저장소 , 의존성을 설정할 수 있으며 `Groovy` 스크립트로 작성되어 있습니다. 이는 Groovy와 Kotlit 언어 중 택하여 개발할 수 있습니다.


![[Gradle2.png]]


### Declaring repositories

repository에는 다양한 종류가 있으며, 형식과 연결 방법으로 구분지을 수 있습니다.

- Format에 따른 구분  
    - Maven 기반 저장소 ( Maven Central , JCenter , Google Android 등 )
    - Ivy 기반 저장소
    - 로컬 디렉토리 형식 저장소
- Connectivity에 따른 구분  
    - 인증 체계가 구성된 저장소 ( BasicAuthentication , DigestAuthentication , HttpHeaderAuthentication 등 인증 체계 )
    - HTTPS , SFTP , AWS S3 , Google Cloud Storage 등 원격 프로토콜 연결 저장소

> repositories {  
> mavenCentral()  
> }

위의 코드는 Gradle이 의존성 모듈을 가져올 저장소를 선언하는 부분으로 Maven Central 저장소에서 의존성을 가져오도록 설정되어 있습니다.

![[Gradle3.png]]



만약 Bintray JCenter , Google Android 저장소에서 가져오고 싶다면 jcenter() , google() 을 활용하여 스크립트를 수정해주세요.

### Dependency Configuration

Gradle 프로젝트에서 선언된 모든 의존성은 사용되는 특정 범위를 가집니다. 의존성의 범위를 표현한 것을 `Dependency Configuration` 이라고합니다.

- Implementation : 구현할 때에만 사용됩니다.
- compileOnly : 컴파일할 때에만 사용하고 런타임에는 사용되지 않습니다.
- runtimeOnly : 런타임 때에만 사용됩니다.
- testImplementation : 테스트할 때에만 사용됩니다.

### Declaring Dependencies

```gradle
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-batch'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
	implementation 'org.springframework.boot:spring-boot-starter-security'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	runtimeOnly 'com.mysql:mysql-connector-j'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	testImplementation 'org.springframework.security:spring-security-test'
}
```

이 부분에서 사용할 의존성을 configuration과 함께 선언해줍니다. Spring Boot 에서는 starter를 통해 버전 관리를 쉽게 할 수 있도록 도와주어 , 해당 의존성을 사용할 때에는 버전을 굳이 명시하지 않아도 됩니다.

### Plugins

```gradle
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.0.4'
	id 'io.spring.dependency-management' version '1.1.0'
}
```

프로젝트에 적용할 플러그인 의존성을 설정하는 코드입니다. 스프링부터 의존성 관리를 해주는 `io.spring.dependency-management` 플러그인과 스프링부트의 버전을 설정해주는 `org.springframework.boot` 플러그인, 언어를 설정하는 `java` 플러그인이 기본적으로 들어가있습니다.

그 외에 id 'eclipse' 를 설정하면 이클립스 IDE 설정파일로 생성해 , 이클립스에 프로젝트를 Import할 수 있게 해줍니다.

### SourceCompatibility

```null
group = 'com.konkuk'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'
```

- group : 프로젝트 생성시의 groupId를 말합니다.
- version : 애플리케이션의 버전 정보로 접미사에 SNAPSHOT이 붙으면 개발 단계라는 의미입니다.  
    - SNAPSHOT : 개발 중
    - M ( Milestone ) : 기능 일부 완성
    - RC ( Release Candidate ) : 테스트가 거의 완료된 개발 버전
- sourceCompatibility : java 소스를 컴파일할 JDK 버전을 정의합니다.
- targetCompatibility : 프로그램이 실행할 수 있는 최소 java 버전을 정의합니다.











---
참조 - https://velog.io/@tjseocld/Gradle-%EC%9D%B4%EB%9E%80