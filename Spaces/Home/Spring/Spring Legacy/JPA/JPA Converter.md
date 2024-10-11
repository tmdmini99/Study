

JPA (Java Persistence API)에서 JSON 데이터를 처리하기 위해서는 `@Converter` 어노테이션을 사용하여 JSON 타입을 Java 객체와 변환하는 사용자 정의 변환기를 만들 수 있습니다. JPA는 기본적으로 JSON 타입을 처리하지 않기 때문에, MySQL, PostgreSQL, Oracle 등 데이터베이스에서 JSON 타입을 지원하더라도, 이를 Java 객체로 매핑하기 위해서 추가적인 작업이 필요합니다.

JsonConverter.java
```java
package com.kpop.merch.common.handler;

import com.google.gson.Gson;
import com.google.gson.JsonElement;
import com.google.gson.JsonSyntaxException;
import com.google.gson.reflect.TypeToken;

import javax.persistence.AttributeConverter;
import javax.persistence.Converter;
import java.util.List;
import java.util.Map;

@Converter(autoApply = false)  // autoApply를 true로 설정하면 모든 엔티티에 자동으로 적용됨
public class JsonConverter implements AttributeConverter<Object, String> {

    private static final Gson gson = new Gson();

    @Override
    public String convertToDatabaseColumn(Object attribute) {
        return attribute == null ? null : gson.toJson(attribute);
    }

    @Override
    public Object convertToEntityAttribute(String dbData) {
        if (dbData == null) {
            return null;  // JSON 문자열이 null인 경우 null 반환
        }
        try {
            JsonElement element = gson.fromJson(dbData, JsonElement.class);

            if (element.isJsonObject()) {
                // JSON이 객체일 경우 Map으로 변환
                return gson.fromJson(dbData, new TypeToken<Map<String, Object>>() {}.getType());
            } else if (element.isJsonArray()) {
                // JSON이 배열일 경우 List로 변환
                return gson.fromJson(dbData, new TypeToken<List<Map<String, Object>>>() {}.getType());
            }
        } catch (JsonSyntaxException e) {
            throw new IllegalStateException("JSON parsing error", e);
        }
        return null;  // 객체나 배열이 아닌 경우 null 반환
    }
}

```


### 2. JPA 엔티티에 JSON 속성 추가

이제 위에서 만든 `JsonConverter`를 JPA 엔티티에서 사용합니다. 아래는 엔티티 클래스에 JSON 속성을 추가하는 방법입니다.

#### 2.1. **MyEntity.java**


```java
package com.kpop.merch.entity;

import com.kpop.merch.common.handler.JsonConverter;

import javax.persistence.*;

@Entity
@Table(name = "my_table")  // 테이블 이름
public class MyEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "data")  // JSON 데이터를 저장할 컬럼
    @Convert(converter = JsonConverter.class)  // 변환기 설정
    private Object data;  // JSON 데이터를 저장할 필드

    // getter 및 setter
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public Object getData() {
        return data;
    }

    public void setData(Object data) {
        this.data = data;
    }
}

```



### 3. JPA Repository 생성

JPA 엔티티에 대한 CRUD 작업을 수행하기 위해 Repository 인터페이스를 생성합니다.

#### 3.1. **MyEntityRepository.java**



```java
package com.kpop.merch.repository;

import com.kpop.merch.entity.MyEntity;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MyEntityRepository extends JpaRepository<MyEntity, Long> {
    // 추가적인 쿼리 메소드를 정의할 수 있습니다.
}

```



MyService.java
```java
package com.kpop.merch.service;

import com.kpop.merch.entity.MyEntity;
import com.kpop.merch.repository.MyEntityRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.HashMap;
import java.util.Map;

@Service
public class MyService {

    @Autowired
    private MyEntityRepository myEntityRepository;

    public void saveData() {
        Map<String, Object> dataMap = new HashMap<>();
        dataMap.put("key1", "value1");
        dataMap.put("key2", "value2");

        MyEntity myEntity = new MyEntity();
        myEntity.setData(dataMap);
        myEntityRepository.save(myEntity);
    }

    public MyEntity getData(Long id) {
        return myEntityRepository.findById(id).orElse(null);
    }
}

```