

`JsonSerializer`는 Gson 라이브러리에서 제공하는 인터페이스로, 객체를 JSON 형식으로 직렬화하는 방법을 정의하는 데 사용됩니다. 이 인터페이스를 구현하면, Gson이 객체를 JSON으로 변환할 때 사용할 사용자 정의 직렬화 로직을 작성할 수 있습니다.

### 주요 기능

- **커스텀 직렬화**: 기본적인 Gson의 직렬화 방법을 오버라이드하여 특정 필드나 타입을 사용자 정의 방식으로 JSON으로 변환할 수 있습니다. 예를 들어, 특정 객체의 필드를 문자열로 변환하거나, 불필요한 필드를 제외하는 등의 작업을 수행할 수 있습니다.

### 사용 방법

1. **`JsonSerializer` 인터페이스 구현**: 이 인터페이스를 구현하는 클래스를 작성합니다. `serialize` 메서드를 오버라이드하여, 원하는 방식으로 JSON으로 변환하는 로직을 작성합니다.
    
2. **Gson에 등록**: 생성된 직렬화기를 `GsonBuilder`를 사용하여 Gson 객체에 등록합니다. 이렇게 하면 Gson이 객체를 JSON으로 변환할 때 이 커스텀 직렬화기를 사용합니다.


**커스텀 직렬화기 정의 (`CustomSerializer`)**

```java
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonSerializationContext;
import com.google.gson.JsonSerializer;

import java.lang.reflect.Type;
import java.util.Map;

public class CustomSerializer implements JsonSerializer<Map<String, Object>> {
    @Override
    public JsonElement serialize(Map<String, Object> src, Type typeOfSrc, JsonSerializationContext context) {
        JsonObject jsonObject = new JsonObject();
        for (Map.Entry<String, Object> entry : src.entrySet()) {
            if ("id".equals(entry.getKey()) && entry.getValue() instanceof Number) {
                // ID를 문자열로 변환하여 저장
                jsonObject.addProperty(entry.getKey(), String.valueOf(entry.getValue()));
            } else {
                jsonObject.add(entry.getKey(), context.serialize(entry.getValue()));
            }
        }
        return jsonObject;
    }
}
```



2. **Gson 객체 생성 (`GsonBuilder`)**
```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;

// Gson 객체 생성
private static final Gson gson = new GsonBuilder()
        .registerTypeAdapter(new TypeToken<Map<String, Object>>(){}.getType(), new CustomSerializer())
        .create();
```



사용 예시
```java
public class JsonbTypeHandler extends BaseTypeHandler<Object> {
    
    // Gson 객체 생성
    private static final Gson gson = new GsonBuilder()
            .registerTypeAdapter(new TypeToken<Map<String, Object>>(){}.getType(), new CustomSerializer())
            .create();

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, Object parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, gson.toJson(parameter)); // 커스텀 직렬화기를 통해 JSON 변환
    }

    // 나머지 메서드는 이전과 동일
}
```



## 또다른 방법


여기서 
```java
package com.kpop.merch.common.handler;  
  
import com.google.gson.Gson;  
import com.google.gson.JsonElement;  
import com.google.gson.JsonSyntaxException;  
import com.google.gson.reflect.TypeToken;  
import org.apache.ibatis.type.BaseTypeHandler;  
import org.apache.ibatis.type.JdbcType;  
  
import java.lang.reflect.Type;  
import java.sql.CallableStatement;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.util.List;  
import java.util.Map;  
  
public class JsonbTypeHandler extends BaseTypeHandler<Object> {  
  
    private static final Gson gson = new Gson();  
  
    @Override  
    public void setNonNullParameter(PreparedStatement ps, int i, Object parameter, JdbcType jdbcType) throws SQLException {  
        ps.setString(i, gson.toJson(parameter));  
    }  
  
    @Override  
    public Object getNullableResult(ResultSet rs, String columnName) throws SQLException {  
        String json = rs.getString(columnName);  
        return parseJson(json);  
    }  
  
    @Override  
    public Object getNullableResult(ResultSet rs, int columnIndex) throws SQLException {  
        String json = rs.getString(columnIndex);  
        return parseJson(json);  
    }  
  
    @Override  
    public Object getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {  
        String json = cs.getString(columnIndex);  
        return parseJson(json);  
    }  
  
    private Object parseJson(String json) {  
        if (json == null) {  
           return null;  // JSON 문자열이 null인 경우 null 반환  
       }  
        try {  
            JsonElement element = gson.fromJson(json, JsonElement.class);  
  
            if (element.isJsonObject()) {  
                // JSON이 객체일 경우 Map으로 변환  
                return gson.fromJson(json, new TypeToken<Map<String, Object>>() {}.getType());  
            } else if (element.isJsonArray()) {  
                // JSON이 배열일 경우 List로 변환  
                return gson.fromJson(json, new TypeToken<List<Map<String, Object>>>() {}.getType());  
            }  
        } catch (JsonSyntaxException e) {  
            throw new IllegalStateException("JSON parsing error", e);  
        }  
        return null;  
    }  
}
```


이렇게 변경 또는 
```java
@Override
public void setNonNullParameter(PreparedStatement ps, int i, Object parameter, JdbcType jdbcType) throws SQLException {
    // ID 값을 문자열로 변환하여 JSON으로 저장
    if (parameter instanceof Map) {
        Map<String, Object> map = (Map<String, Object>) parameter;
        if (map.containsKey("id") && map.get("id") instanceof Number) {
            map.put("id", String.valueOf(map.get("id"))); // ID를 문자열로 변환하여 저장
        }
    }
    ps.setString(i, gson.toJson(parameter));
}

```

이렇게 변경
```java
private Object parseJson(String json) {
    if (json == null) {
        return null; // JSON 문자열이 null인 경우 null 반환
    }
    try {
        JsonElement element = gson.fromJson(json, JsonElement.class);

        if (element.isJsonObject()) {
            // JSON이 객체일 경우 Map으로 변환
            Map<String, Object> map = gson.fromJson(json, new TypeToken<Map<String, Object>>() {}.getType());
            if (map.containsKey("id") && map.get("id") instanceof Number) {
                map.put("id", String.valueOf(map.get("id"))); // ID를 문자열로 변환
            }
            return map;
        } else if (element.isJsonArray()) {
            // JSON이 배열일 경우 List로 변환
            return gson.fromJson(json, new TypeToken<List<Map<String, Object>>>() {}.getType());
        }
    } catch (JsonSyntaxException e) {
        throw new IllegalStateException("JSON parsing error", e);
    }
    return null;
}

```