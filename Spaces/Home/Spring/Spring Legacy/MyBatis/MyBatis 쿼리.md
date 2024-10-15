

파라미터 타입 기본 타입 지정시 
```xml
parameterType="java.lang.String"
```


config.xml  camelCase 지정
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <settings>  
        <setting name="mapUnderscoreToCamelCase" value="true" />  
        <setting name="jdbcTypeForNull" value="NULL" />  
        <setting name="logImpl" value="SLF4J"/> <!-- Log4j2, STDOUT_LOGGING 등 다른 로깅 프레임워크도 가능 -->  
    </settings>  
</configuration>
```

