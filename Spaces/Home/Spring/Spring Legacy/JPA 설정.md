#### **1. Project Facets JPA 추가**

먼저 이전에 만들었던 프로젝트를 JPA 프로젝트로 바꿔야한다.

프로젝트 우클릭 > Properties > Project Facets > JPA체크 후 Apply

처음에는 어떤 경고문구가 있었던거 같은데 기억이 안나지만, 아무 설정 없는채로 일단 만들면 적용이 되었던 것 같다.

Convert가 완료 되면 src/main/java에 META-INF가 생기고 그 폴더 안에 persistence.xml이 생겨있다.

pom.xml
```xml
<!-- JPA -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-jpa</artifactId>
			<version>2.2.1.RELEASE</version>
		</dependency>
		
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<version>5.1.11.Final</version>
		</dependency>
	</dependencies>
```







---
참조 - https://glow153.tistory.com/25