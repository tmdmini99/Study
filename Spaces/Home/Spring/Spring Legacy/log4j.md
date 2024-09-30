
SLF4J API만으로는 로그를 출력할 수 없음.  SLF4J를 사용할 때는 반드시 구현체가 필요.

pom.xml
```xml

<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.32</version>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.6</version>
</dependency>

```



log4j.xml
```xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%date %level [%thread] %logger{10} [%file:%line] %msg%n</pattern>
        </encoder>
    </appender>

    <logger name="org.mybatis" level="DEBUG"/>
    <logger name="jdbc.sql" level="DEBUG"/>

    <root level="DEBUG">
        <appender-ref ref="STDOUT"/>
    </root>
</configuration>

```



mybatis-config.xml
```xml
<configuration>
    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true" />
        <setting name="jdbcTypeForNull" value="NULL" />
        <setting name="logImpl" value="SLF4J"/> <!-- SLF4J를 로그 구현체로 사용 -->
    </settings>
</configuration>

```


