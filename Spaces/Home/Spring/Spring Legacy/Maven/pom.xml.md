
Spring Lecay pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0</modelVersion>  
  
    <groupId>org.example</groupId>  
    <artifactId>OpenApiMyBatis</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  
    <properties>  
        <maven.compiler.source>8</maven.compiler.source>  
        <maven.compiler.target>8</maven.compiler.target>  
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
        <org.springframework-version>5.2.11.RELEASE</org.springframework-version>  
        <org.slf4j-version>1.7.5</org.slf4j-version>  
        <org.aspectj-version>1.6.10</org.aspectj-version>  
    </properties>  
    <dependencies>  
        <!-- Spring -->  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-context</artifactId>  
            <version>${org.springframework-version}</version>  
            <exclusions>  
                <!-- Exclude Commons Logging in favor of SLF4j -->  
                <exclusion>  
                    <groupId>commons-logging</groupId>  
                    <artifactId>commons-logging</artifactId>  
                </exclusion>  
            </exclusions>  
        </dependency>  
  
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-beans</artifactId>  
            <version>${org.springframework-version}</version>  
        </dependency>  
  
  
  
        <!-- Servlet -->  
        <dependency>  
            <groupId>javax.servlet</groupId>  
            <artifactId>servlet-api</artifactId>  
            <version>2.5</version>  
            <scope>provided</scope>  
        </dependency>  
        <dependency>  
            <groupId>javax.servlet.jsp</groupId>  
            <artifactId>jsp-api</artifactId>  
            <version>2.1</version>  
            <scope>provided</scope>  
        </dependency>  
        <dependency>  
            <groupId>javax.servlet</groupId>  
            <artifactId>jstl</artifactId>  
            <version>1.2</version>  
        </dependency>  
  
        <!-- Test -->  
        <dependency>  
            <groupId>junit</groupId>  
            <artifactId>junit</artifactId>  
            <version>4.7</version>  
            <scope>test</scope>  
        </dependency>  
  
  
        <!--       &lt;!&ndash; https://mvnrepository.com/artifact/org.mybatis/mybatis &ndash;&gt;   -->        <dependency>  
            <groupId>org.mybatis</groupId>  
            <artifactId>mybatis</artifactId>  
            <version>3.5.10</version>  
        </dependency>  
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring &ndash;&gt;       -->        <dependency>  
            <groupId>org.mybatis</groupId>  
            <artifactId>mybatis-spring</artifactId>  
            <version>2.0.7</version>  
        </dependency>  
  
  
  
<!--        &lt;!&ndash; JPA &ndash;&gt;        <dependency>-->  
<!--        <groupId>org.springframework</groupId>-->  
<!--        <artifactId>spring-orm</artifactId>-->  
<!--        <version>${org.springframework-version}</version>-->  
<!--    </dependency>-->  
  
<!--        <dependency>-->  
<!--            <groupId>org.springframework.data</groupId>-->  
<!--            <artifactId>spring-data-jpa</artifactId>-->  
<!--            <version>2.2.1.RELEASE</version>-->  
<!--        </dependency>-->  
  
  
<!--        <dependency>-->  
<!--            <groupId>org.hibernate</groupId>-->  
<!--            <artifactId>hibernate-entitymanager</artifactId>-->  
<!--            <version>5.1.11.Final</version>-->  
<!--        </dependency>-->  
        <!-- -->        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-jdbc</artifactId>  
            <version>${org.springframework-version}</version>  
        </dependency>  
        <!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->  
        <dependency>  
            <groupId>com.mysql</groupId>  
            <artifactId>mysql-connector-j</artifactId>  
            <version>8.2.0</version>  
        </dependency>  
  
        <!-- Logging -->  
        <dependency>  
            <groupId>org.slf4j</groupId>  
            <artifactId>slf4j-api</artifactId>  
            <version>${org.slf4j-version}</version>  
        </dependency>  
  
        <!-- https://mvnrepository.com/artifact/log4j/log4j -->  
        <dependency>  
            <groupId>log4j</groupId>  
            <artifactId>log4j</artifactId>  
            <version>1.2.17</version>  
        </dependency>  
        <dependency>  
            <groupId>ch.qos.logback</groupId>  
            <artifactId>logback-classic</artifactId>  
            <version>1.2.3</version>  
        </dependency>  
  
  
        <!-- @Inject   -->  
        <dependency>  
            <groupId>javax.inject</groupId>  
            <artifactId>javax.inject</artifactId>  
            <version>1</version>  
        </dependency>  
        <!-- Lombok-->  
        <dependency>  
            <groupId>org.projectlombok</groupId>  
            <artifactId>lombok</artifactId>  
            <version>1.18.12</version>  
        </dependency>  
        <!-- gson-->  
        <dependency>  
            <groupId>com.google.code.gson</groupId>  
            <artifactId>gson</artifactId>  
            <version>2.8.5</version>  
        </dependency>  
  
  
  
        <!-- AspectJ -->  
        <dependency>  
            <groupId>org.aspectj</groupId>  
            <artifactId>aspectjrt</artifactId>  
            <version>${org.aspectj-version}</version>  
        </dependency>  
  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-web</artifactId>  
            <version>${org.springframework-version}</version>  
        </dependency>  
  
        <dependency>  
            <groupId>org.springframework</groupId>  
            <artifactId>spring-webmvc</artifactId>  
            <version>${org.springframework-version}</version>  
        </dependency>  
  
  
    </dependencies>  
    <build>  
        <plugins>  
  
            <plugin>  
                <groupId>org.apache.maven.plugins</groupId>  
                <artifactId>maven-compiler-plugin</artifactId>  
                <version>2.5.1</version>  
                <configuration>  
                    <source>1.6</source>  
                    <target>1.6</target>  
                    <compilerArgument>-Xlint:all</compilerArgument>  
                    <showWarnings>true</showWarnings>  
                    <showDeprecation>true</showDeprecation>  
                </configuration>  
            </plugin>  
            <plugin>  
                <groupId>org.codehaus.mojo</groupId>  
                <artifactId>exec-maven-plugin</artifactId>  
                <version>1.2.1</version>  
                <configuration>  
                    <mainClass>org.test.int1.Main</mainClass>  
                </configuration>  
            </plugin>  
        </plugins>  
    </build>  
  
  
  
</project>
```



---

참조 ---
https://riverblue.tistory.com/35