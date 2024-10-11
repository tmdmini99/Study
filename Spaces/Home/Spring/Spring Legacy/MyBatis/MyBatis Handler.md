

mybatis-config.xml에서 이렇게 설정 필요

```xml
<typeHandlers>  
    <typeHandler javaType="java.util.Map" jdbcType="OTHER" handler="com.kpop.merch.common.handler.JsonbTypeHandler"/>  
    <typeHandler javaType="java.util.List" jdbcType="OTHER" handler="com.kpop.merch.common.handler.JsonbTypeHandler"/>  
</typeHandlers>
```





```java
package com.kpop.merch.orders.vo;  
  
import lombok.Data;  
  
import java.util.List;  
import java.util.Map;  
  
@Data  
public class OrdersVo {  
   
    private Map<String, Object> billingAddress;  
    private Map<String, Object> customer;  
    private List<Map<String, Object>> fulfillments;  
    private List<Map<String, Object>> lineItems;  
}

```

배열일 경우

```java
private List<Map<String, Object>> fulfillments;
```

일반 json일 경우
```java
private Map<String, Object> customer;
```





## PostGre

json만 처리
```java
package com.kpop.merch.common.handler;  
  
import com.google.gson.Gson;  
import com.google.gson.reflect.TypeToken;  
import org.apache.ibatis.type.BaseTypeHandler;  
import org.apache.ibatis.type.JdbcType;  
  
import java.lang.reflect.Type;  
import java.sql.CallableStatement;  
import java.sql.PreparedStatement;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.util.Map;  
  
public class JsonbTypeHandler extends BaseTypeHandler<Map<String, Object>> {  
  
    private static final Gson gson = new Gson();  
    private static final Type type = new TypeToken<Map<String, Object>>(){}.getType();  
  
    @Override  
    public void setNonNullParameter(PreparedStatement ps, int i, Map<String, Object> parameter, JdbcType jdbcType) throws SQLException {  
        ps.setString(i, gson.toJson(parameter));  
    }  
  
    @Override  
    public Map<String, Object> getNullableResult(ResultSet rs, String columnName) throws SQLException {  
        String json = rs.getString(columnName);  
        return json == null ? null : gson.fromJson(json, type);  
    }  
  
    @Override  
    public Map<String, Object> getNullableResult(ResultSet rs, int columnIndex) throws SQLException {  
        String json = rs.getString(columnIndex);  
        return json == null ? null : gson.fromJson(json, type);  
    }  
  
    @Override  
    public Map<String, Object> getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {  
        String json = cs.getString(columnIndex);  
        return json == null ? null : gson.fromJson(json, type);  
    }  
}
```



배열만 처리
```java
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.sql.*;
import java.util.List;
import java.util.Map;

public class JsonbArrayTypeHandler extends BaseTypeHandler<List<Map<String, Object>>> {

    private static final Gson gson = new Gson();

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, List<Map<String, Object>> parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, gson.toJson(parameter));
    }

    @Override
    public List<Map<String, Object>> getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String json = rs.getString(columnName);
        return parseJsonArray(json);
    }

    @Override
    public List<Map<String, Object>> getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String json = rs.getString(columnIndex);
        return parseJsonArray(json);
    }

    @Override
    public List<Map<String, Object>> getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String json = cs.getString(columnIndex);
        return parseJsonArray(json);
    }

    private List<Map<String, Object>> parseJsonArray(String json) {
        return gson.fromJson(json, new TypeToken<List<Map<String, Object>>>(){}.getType());
    }
}

```


```java
import org.json.JSONObject;
import java.util.Map;

public class YourVO {
    private int id;
    private String name;
    private Map<String, Object> jsonbColumn;

    // Setter에서 JSON 문자열을 Map으로 변환
    public void setJsonbColumn(String jsonbColumnString) {
        JSONObject jsonObject = new JSONObject(jsonbColumnString);
        this.jsonbColumn = jsonObject.toMap();  // org.json의 toMap()을 사용
    }

    public Map<String, Object> getJsonbColumn() {
        return jsonbColumn;
    }
}

```


배열과 일반 json 모두 처리
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


그 후 이렇게 설정하면 된다
```jsp
<c:forEach var="list" items="${list}" varStatus="status">  
    <tr class="align-middle">  
        <td>${status.count}</td>  
        <td>${list.name}</td>  
        <td>${list.customer['last_name']}${list.customer['fisrt_name']}</td>  
    </tr>  
</c:forEach>
```



```jsp
<c:choose>
    <c:when test="${jsonbColumn instanceof java.util.List}">
        <!-- 배열로 처리 -->
        <c:forEach var="item" items="${jsonbColumn}">
            ${item.name} - ${item.status}
        </c:forEach>
    </c:when>
    <c:otherwise>
        <!-- 객체로 처리 -->
        ${jsonbColumn.name} - ${jsonbColumn.status}
    </c:otherwise>
</c:choose>

```


삼항 연산자 여러개
```jsp
<td>  
    ${list.sourceName == 'web' ? 'Online Store' : (list.sourceName == '3890849' ? 'Shop' : '발주 주문')}  
</td>
```

```jsp
<td>
    <c:choose>
        <c:when test="${list.sourceName == 'web'}">
            Online Store
        </c:when>
        <c:when test="${list.sourceName == '3890849'}">
            Shop
        </c:when>
        <c:when test="${list.sourceName == 'shopify_draft_order'}">
            발주 주문
        </c:when>
    </c:choose>
</td>
```




```jsp
<c:choose>
	<c:when test="${list.shippingLines == null or empty list.shippingLines}">
		배송 정보 없음
	</c:when>
	<c:otherwise>
		배송 정보가 존재합니다.
	</c:otherwise>
</c:choose>
```






## Oracle


Oracle은 **JSON을 `VARCHAR` 또는 `CLOB`** 필드에 저장합니다. Oracle 12c 이상에서는 **JSON을 위한 기능**이 내장되어 있지만, 실제 저장은 문자열 형식입니다. Oracle에서도 JSON 처리를 위해 비슷한 타입 핸들러를 사용할 수 있습니
다.



```sql
CREATE TABLE my_table (
    id INT PRIMARY KEY,
    data JSON  -- MySQL에서 JSON 타입 사용
);

```


```java
package com.kpop.merch.common.handler;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.lang.reflect.Type;
import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Map;

public class OracleJsonTypeHandler extends BaseTypeHandler<Map<String, Object>> {

    private static final Gson gson = new Gson();
    private static final Type type = new TypeToken<Map<String, Object>>(){}.getType();

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, Map<String, Object> parameter, JdbcType jdbcType) throws SQLException {
        // Oracle에서는 JSON 데이터를 VARCHAR 또는 CLOB에 저장
        ps.setString(i, gson.toJson(parameter));
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String json = rs.getString(columnName);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String json = rs.getString(columnIndex);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String json = cs.getString(columnIndex);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }
}

```


Oracle에서도 **JSON 데이터를 `VARCHAR` 또는 `CLOB`**로 저장하며, JSON 데이터를 처리할 때는 MyBatis가 이 핸들러를 통해 **문자열**로 변환하여 저장하고, 조회할 때는 **문자열을 Map으로 변환**하여 Java 객체로 사용할 수 있습니다.


**VO (Value Object)**: 특별한 처리를 할 필요가 없으며, JSON 데이터를 담기 위해 `Map<String, Object>`를 사용.




## MySql


MySQL은 **JSON** 타입을 지원하므로, JSON 데이터를 특별히 처리할 필요가 없고, `VARCHAR`나 `TEXT`와 비슷하게 사용할 수 있습니다.


```java
package com.kpop.merch.common.handler;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.lang.reflect.Type;
import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Map;

public class JsonTypeHandler extends BaseTypeHandler<Map<String, Object>> {

    private static final Gson gson = new Gson();
    private static final Type type = new TypeToken<Map<String, Object>>(){}.getType();

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, Map<String, Object> parameter, JdbcType jdbcType) throws SQLException {
        // MySQL의 JSON 필드는 문자열로 처리됨
        ps.setString(i, gson.toJson(parameter));
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String json = rs.getString(columnName);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String json = rs.getString(columnIndex);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String json = cs.getString(columnIndex);  // JSON을 문자열로 가져옴
        return json == null ? null : gson.fromJson(json, type);  // Map으로 변환
    }
}

```


- **MySQL의 JSON 필드**는 문자열로 변환되며, `VARCHAR`처럼 `gson.toJson()`을 사용하여 JSON 문자열로 변환한 뒤, MySQL의 JSON 필드에 저장됩니다.
- 데이터를 조회할 때는 `getString()`을 사용하여 **JSON 문자열**을 받아와 `gson.fromJson()`으로 **Java의 Map** 객체로 변환합니다.

vo
```java
public class MyDataVO {
    private int id;
    private Map<String, Object> data;  // JSON 데이터를 저장할 Map

    // getter, setter
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public Map<String, Object> getData() {
        return data;
    }

    public void setData(Map<String, Object> data) {
        this.data = data;
    }
}

```


```sql
CREATE TABLE my_table (
    id INT PRIMARY KEY,
    data JSON  -- MySQL의 JSON 타입 사용
);

```


### MySQL에서 VARCHAR 또는 TEXT로 JSON을 처리하는 경우




```java
public class MyDataVO {
    private int id;
    private Map<String, Object> data;  // JSON 데이터를 저장할 Map

    // getter, setter
}

```


mapper
```xml
<insert id="insertMyData">
    INSERT INTO my_table (id, data)
    VALUES (#{id}, #{data, typeHandler=com.kpop.merch.common.handler.JsonTypeHandler})
</insert>

<select id="selectMyDataById" resultType="MyDataVO">
    SELECT id, data
    FROM my_table
    WHERE id = #{id}
</select>

```



```java
package com.kpop.merch.common.handler;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.lang.reflect.Type;
import java.sql.*;
import java.util.Map;

public class JsonTypeHandler extends BaseTypeHandler<Map<String, Object>> {

    private static final Gson gson = new Gson();
    private static final Type type = new TypeToken<Map<String, Object>>() {}.getType();

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, Map<String, Object> parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, gson.toJson(parameter));  // Map을 JSON 문자열로 변환하여 저장
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String json = rs.getString(columnName);
        return json == null ? null : gson.fromJson(json, type);  // JSON 문자열을 Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String json = rs.getString(columnIndex);
        return json == null ? null : gson.fromJson(json, type);  // JSON 문자열을 Map으로 변환
    }

    @Override
    public Map<String, Object> getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String json = cs.getString(columnIndex);
        return json == null ? null : gson.fromJson(json, type);  // JSON 문자열을 Map으로 변환
    }
}

```