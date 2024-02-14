
log4j.xml에 ConsoleAppender하나 더 추가
```xml
<appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out" />
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d %5p [%c] %m%n" />
    </layout>
</appender>

<appender name="console-infolog" class="org.apache.log4j.ConsoleAppender">
    <layout class="org.apache.log4j.PatternLayout">
        <param name="ConversionPattern" value="%d %5p %m%n"/>
    </layout>
</appender>

```

`console` Appender는 로그 메시지에 클래스 이름(`[%c]`)을 추가하여 출력하고, `console-infolog` Appender는 클래스 이름 없이 간단한 형식으로 출력할 수 있습니다.

```
2024-02-14 12:34:56 INFO [com.example.MyClass] This is an info message.
2024-02-14 12:34:57 WARN [com.example.MyClass] This is a warning message.
2024-02-14 12:34:58 ERROR [com.example.MyClass] This is an error message.
2024-02-14 12:34:59 INFO This is a simple info message.
2024-02-14 12:35:00 WARN This is a simple warning message.
2024-02-14 12:35:01 ERROR This is a simple error message.

```

log4j.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//APACHE//DTD LOG4J 1.2//EN" "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">

	<!-- Appenders -->
	<appender name="console" class="org.apache.log4j.ConsoleAppender">
		<param name="Target" value="System.out" />
		<layout class="org.apache.log4j.PatternLayout">
			<param name="ConversionPattern" value="%-5p: %c - %m%n" />
		</layout>
	</appender>
	
	<!-- Application Loggers -->
	<logger name="com.iu.s1">
		<level value="info" />
	</logger>
	
	<!-- 3rdparty Loggers -->
	<logger name="org.springframework.core">
		<level value="info" />
	</logger>
	
	<logger name="org.springframework.beans">
		<level value="info" />
	</logger>
	
	<logger name="org.springframework.context">
		<level value="info" />
	</logger>

	<logger name="org.springframework.web">
		<level value="info" />
	</logger>

	<!-- Root Logger -->
	<root>
		<priority value="warn" />
		<appender-ref ref="console" />
	</root>
	
</log4j:configuration>
```



pom.xml
slf4j 포함

```xml
<org.slf4j-version>1.6.6</org.slf4j-version>

<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
</dependency>
<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
</dependency>
<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>
```

exclusions - 종속성 제거
mail과 jms jmxtools jmxri를 다운하지 않음








---
참조 - https://to-dy.tistory.com/20