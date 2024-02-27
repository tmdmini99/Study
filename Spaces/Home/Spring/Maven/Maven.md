## Maven 이란?

Maven이란 자바용 프로젝트 관리 도구이다.

### 빌드란?

프로젝트를 위해 작성한 Java코드나 여러 자원들(.xml, .jar, .properties)를 JVM이나 톰캣같은 WAS가 인식할 수 있는 구조로 패키징 하는 과정 및 결과물이다.  
또 단순히 컴파일해주는 작업 뿐만 아니라, 테스팅, 검사, 배포까지 일련의 작업들을 통틀어 빌드라고 한다.

## Maven



Maven은 Apache사에서 만든 빌드툴(build tool)이다.  
pom.xml파일을 통해 정형화된 빌드 시스템으로 프로젝트 관리를 해준다.  
프로젝트의 전체적인 라이프 사이클을 관리한다.

### 특징

- 빌드 과정을 쉽게 만들기
- 정형화된 빌드 시스템 제공
- Maven은 POM과 플러그인 세트를 사용하여 프로젝트를 빌드한다.
- 양질의 프로젝트 정보 제공
- 더 나은 개발

### 장점

- 편리한 의존성 라이브러리 관리
- 정해진 빌드 방법을 사용하여 협업에서 유리하게 작용
- 다양한 플러그인을 통해 많은 작업이 자동화됨

플러그인을 구동해주는 프레임워크로, 모든 작업은 플로그인에서 수행하게된다.



![[Maven1.png]]


## Maven LifeCycle


_**1) LifeCycle**_   
 - 미리 정해진 빌드순서   
 - 메이븐은 프레임워크이기 때문에 동작 방식이 정해져있고, 미리 정의하고 있는 빌드 순서가 있다. 이를 라이프사이클(Lifecycle)이라 한다. 

![[Maven4.png]]

Default(Build) : 일반적인 빌드 프로세스를 위한 모델이다.   
◎ Clean : 빌드 시 생성되었던 파일들을 삭제하는 단계   
◎ Validate : 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인하는 단계   
◎ Compile : 프로젝트의 소스코드를 컴파일 하는 단계   
◎ Test : 유닛(단위) 테스트를 수행 하는 단계(테스트 실패시 빌드 실패로 처리, 스킵 가능)   
◎ Pacakge : 실제 컴파일된 소스 코드와 리소스들을 jar, war 등등의 파일 등의 배포를 위한 패키지로 만드는 단계   
◎ Verify : 통합 테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계   
◎ Install : 패키지를 로컬 저장소에 설치하는 단계   
◎ Site : 프로젝트 문서와 사이트 작성, 생성하는 단계   
◎ Deploy : 만들어진 package를 원격 저장소에 release 하는 단계 

최종 빌드 순서는 compile => test => package 이다.    
① compile : src/main/java 디렉토리 아래의 모든 소스 코드가 컴파일 된다.   
② test : src/test/java, src/test/resources 테스트 자원 복사 및 테스트 소스 코드 컴파일 된다. 

     ※ junit : 단위 테스트 프레임워크. 테스트 단계를 거치기 위해 의존 설정을 해준다.   
③ packaging : 컴파일과 테스트가 완료 된 후, jar, war 같은 형태로 압축하는 작업.   
  
_**2) Phase(단계)**_   
Build Lifecycle의 각각의 단계를 Phase라고 한다.   
Phase는 의존관계를 가지고 있어 해당 Phase가 수행되려면 이전 단계의 Phase가 모두 수행되어야 한다.   
즉, 모든 빌드단계는 이전 단계가 성공적으로 실행되었을 때 실행된다는 것이 Dependency 입니다.   
  
_**3) Goal**_   
 - 특정 작업, 최소한의 실행 단위(task).   
 - 하나의 플러그인에서는 여러 작업을 수행할 수 있도록 지원하며, 플러그인에서 실행할 수 있는 각각의 기능(명령)을 Goal이라고 한다.   
(각각의 Phase에 연계된 Goal을 실행하는 과정을 Build라고 한다.)

 - 플러그인의 goal을 실행하는 방법은 다음과 같다.   
 ■ - mvn groupId:artifactId:version:goal(아래와 같이 생략 가능)   
■ - mvn plugin:goal




![[Maven2.png]]


메이븐은 정해진 라이프 사이클을 통해 프로젝트를 빌드한다.  
메이븐 라이프 사이클의 종류는 기본, clean, site가 있다.  
각 라이프 사이클 안에는 더 작은 단위의 빌드 단계가 정의되어 있는데 이를 phase라고 한다.

![[Maven3.jpg]]

`phase`는 논리적인 빌드 단계이고, 실제로는 `phase`에 연결된 `plug-in`있고 `plug-in`이 수행하는 명령을 `goal`이라고 한다.


`요약하자면 빌드 순서는 Compile - Test - Package 이다.`  
Clean -> init -> compile -> test-compile -> test -> package -> integration-test -> verify -> install -> deploy -> site

## Maven 설정 파일

### settings.xml

- 메이븐 빌드 툴과 관련된 설정파일
-  MAVEN_HOME/conf 디렉토리에 위치 (메이븐 설치 시 기본 제공)

- settings.xml의  설정   
     
** 메이븐을 빌드할 때 의존 관계에 있는 라이브러리, 플러그인을 중앙 저장소에서 개발자 PC로 다운로드 하는위치(로컬저장소)의 기본 설정 'USER_HOME/.m2/repository' 인데 settings.xml의 에 원하는 로컬 저장소의 경로를 지정, 변경할 수 있다.




### POM

프로젝트마다 하나의 pom.xml파일이 있다.  
프로젝트의 모든 설정, 의존성 등을 설정할 수 있다.  

 POM은 pom.xml파일을 말하며 pom.xml은 메이븐을 이용하는 프로젝트의 root에 존재하는 xml 파일이다.   
   (하나의 자바 프로젝트에 빌드 툴을 maven으로 설정하면, 프로젝트 최상위 디렉토리에 "pom.xml"이라는 파일이 생성된다.)   
 - Maven의 기능을 이용하기 위해서 POM이 사용된다.    
 - 파일은 프로젝트마다 1개이며, pom.xml만 보면 프로젝트의 모든 설정, 의존성 등을 알 수 있다.   
 - 다른 파일이름으로 지정할 수도 있다. (mvn -f 파일명.xml test). 하지만 pom.xml으로 사용하기를 권장한다.





#### pom.xml 엘리트먼트


```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion> <!--POM model의 버전-->
	<parent> <!--프로젝트의 계층 정보-->
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.god</groupId> <!--프로젝트를 생성하는 조직의 고유 아이디를 결정한다. 일반적으로 도메인 이름을 거꾸로 적는다.-->
	<artifactId>bo</artifactId> <!--프로젝트 빌드시 파일 대표이름 이다. groupId 내에서 유일해야 한다.Maven을 이용하여 빌드시 다음과 같은 규칙으로 파일이 생성 된다.
		artifactid-version.packaging. 위 예의 경우 빌드할 경우 bo-0.0.1-SNAPSHOT.war 파일이 생성된다.-->
	<version>0.0.1-SNAPSHOT</version> <!--프로젝트의 현재 버전, 프로젝트 개발 중일 때는 SNAPSHOT을 접미사로 사용-->
	<packaging>war</packaging> <!--패키징 유형(jar, war, ear 등)-->
	<name>bo</name> <!--프로젝트, 프로젝트 이름-->
	<description>Demo project for Spring Boot</description> <!--프로젝트에 대한 간략한 설명-->
	<url>http://goddaehee.tistory.com</url> <!--프로젝트에 대한 참고 Reference 사이트-->

	<properties>
	<!-- 버전관리시 용이 하다. ex) 하당 자바 버전을 선언 하고 dependencies에서 다음과 같이 활용 가능 하다.
	<version>${java.version}</version> -->
		<java.version>1.8</java.version>
	</properties>

	<dependencies> <!--dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리 한다.-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build> <!--빌드에 사용할 플러그인 목록-->
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```



- project : pom.xml 파일의 최상위 루트 엘리먼트(Root Element)입니다.
-  parent : 프로젝트의 계층 정보
- modelVersion : POM model의 버전입니다.
- groupId : 프로젝트를 생성하는 조직의 고유 아이디를 결정합니다. 일반적으로 도메인 이름을 거꾸로 적습니다.
- artifactId : 해당 프로젝트에 의하여 생성되는 artifact의 고유 아이디를 결정합니다. Maven을 이용하여 pom.xml을 빌드할 경우 다음과 같은 규칙으로 artifact가 생성됩니다. artifactid-version.packaging. 위 예의 경우 빌드할 경우 examples-1.0-SNAPSHOT.jar 파일이 생성됩니다.
- packaging : 해당 프로젝트를 어떤 형태로 packaging 할 것인지 결정합니다. jar, war, ear 등이 해당됩니다.  
    version : 프로젝트의 현재 버전. 추후 살펴보겠지만 프로젝트가 개발 중일 때는 SNAPSHOT을 접미사로 사용합니다. Maven의 버전 관리 기능은 라이브러리 관리를 편하게 합니다.
- name : 프로젝트의 이름입니다.
- url : 프로젝트 사이트가 있다면 사이트 URL을 등록하는 것이 가능합니다.
- description : 프로젝트에 대한 간략한 설명
- properties : 버전관리시 용이 하다. ex) 하당 자바 버전을 선언 하고 dependencies에서 다음과 같이 활용 가능 하다
```xml
<version>${java.version}</version>
```

dependencies : dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리 한다.

 build : 빌드에 사용할 플러그인 목록


해당 엘리먼트 안에 필요한 라이브러리를 지정하게 됩니다.





---
참조 - https://velog.io/@changyeonyoo/Maven-%EC%9D%B4%EB%9E%80

https://goddaehee.tistory.com/199
