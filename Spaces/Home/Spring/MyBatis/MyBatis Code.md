
자동증가 값 가져오기

```xml
<insert id="insertCategory" parameterType="map" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO board_category (kr_name, en_name, distinction)
    VALUES (#{krName}, #{enName}, 'notice');
</insert>
```


- **`useGeneratedKeys="true"`**:
    - MyBatis에게 자동 생성된 키 값을 가져오도록 지시합니다.
- **`keyProperty="id"`**:
    - 매핑될 객체 또는 파라미터에서 자동 생성된 키 값을 저장할 속성 또는 필드 이름을 지정합니다.
    - 여기서 `id`는 입력 파라미터나 매핑 객체의 필드 이름이어야 합니다.


```java
Map<String, Object> param = new HashMap<>();
param.put("krName", "카테고리 이름");
param.put("enName", "Category Name");

// insert 수행
sqlSession.insert("namespace.insertCategory", param);

// 자동 생성된 ID 가져오기
Integer generatedId = (Integer) param.get("id");
System.out.println("Generated ID: " + generatedId);
```



## parameterType="map"


MyBatis에서는 `parameterType="map"`을 사용하면, **자동으로** 객체를 `Map`으로 변환하여 SQL 쿼리와 매핑해줍니다.

### 왜 자동으로 변환되냐면?

MyBatis는 **객체(VO)를 `Map`으로 변환**하는 기능을 내장하고 있습니다. 기본적으로 MyBatis는 **Java 객체의 필드**와 **SQL 쿼리에서 사용하는 변수**를 매핑할 때, **리플렉션**을 사용해서 객체의 값을 `Map` 형태로 추출합니다. 그래서 특별한 설정 없이 `parameterType="map"`을 사용하면, MyBatis가 **자동으로 객체의 필드 값을 `Map`으로 추출하여** 쿼리에서 사용하는 변수로 바인딩합니다.

### 예시:

1. **Java 객체 (VO)**

```java
public class BasicCUDParamVo {
    private String userId;
    private String title;
    private String contents;
    private String categoryId;
    private String url;
    private String fileId;

    // getters and setters
}
```


Mapper XML에서 `parameterType="map"` 사용
```xml
<insert id="insert" parameterType="map">
    INSERT INTO board_notice (user_id, title, contents, category_id, url, file_id)
    VALUES (#{user_id}, #{title}, #{contents}, #{category_id}, #{url}, #{file_id})
</insert>
```

Java 코드에서 VO 객체를 전달
```java
BasicCUDParamVo paramVo = new BasicCUDParamVo();
paramVo.setUserId("user123");
paramVo.setTitle("New Title");
paramVo.setContents("This is the content.");
paramVo.setCategoryId("cat001");
paramVo.setUrl("http://example.com");
paramVo.setFileId("file123");

int result = sqlSession.insert("KWM3100Mapper.insert", paramVo);
```



### 자동 변환 과정:

- MyBatis는 `paramVo` 객체를 **리플렉션을 통해** 분석하여 각 필드 값을 `Map`으로 변환합니다.
    
    - 예를 들어 `paramVo.getUserId()` 메서드를 호출하여 `"user123"` 값을 추출하고, 이를 `Map<String, Object>` 형태로 추가합니다.
    - 변환된 `Map`은 SQL 쿼리에서 `#{user_id}`, `#{title}` 등의 바인딩 변수에 전달됩니다.

```java
Map<String, Object> paramMap = new HashMap<>();
paramMap.put("user_id", "user123");
paramMap.put("title", "New Title");
paramMap.put("contents", "This is the content.");
paramMap.put("category_id", "cat001");
paramMap.put("url", "http://example.com");
paramMap.put("file_id", "file123");
```


- **자동으로 변환됩니다**. MyBatis는 `parameterType="map"`으로 설정된 경우, Java 객체를 **자동으로 `Map`으로 변환**하여 SQL 쿼리와 매핑합니다.
- 그래서 **`BasicCUDParamVo`와 같은 객체를** 직접 전달해도 MyBatis는 내부적으로 **리플렉션을 통해 자동으로 `Map`으로 변환**하여 처리합니다.
 - MyBatis는 `parameterType="map"`을 사용하면 `java.util.Map` 객체를 매핑 대상으로 인식합니다



```xml
<choose>  
    <when test="sc_ROUTE_RUN_TYPE != '' and sc_ROUTE_RUN_TYPE != null">  
       AND ROUTE_DTY = #{sc_ROUTE_RUN_TYPE}  
    </when>  
    <otherwise>  
       AND ROUTE_DTY != '30'  
    </otherwise>  
</choose>
```

mybatis에서 if else if 사용법




MyBatis에서 `for`문을 사용하려면 `<foreach>` 태그를 활용합니다. `<foreach>`는 SQL에서 반복문을 구현하기 위한 MyBatis의 태그로, 주로 리스트나 배열 같은 컬렉션을 처리할 때 사용됩니다.

---

### 기본 문법


```xml
<foreach 
    collection="list" 
    item="item" 
    index="index" 
    open="(" 
    separator="," 
    close=")">
    #{item}
</foreach>
```


#### 속성 설명

- **`collection`**: 반복할 데이터 컬렉션의 이름. Java의 `List`, `Set`, `Map`, 배열 등이 가능합니다.
- **`item`**: 컬렉션의 각 요소를 참조할 변수 이름.
- **`index`**: (선택 사항) 컬렉션의 인덱스를 참조할 변수 이름.
- **`open`**: 반복문 시작 전에 추가할 문자열.
- **`separator`**: 각 요소 사이에 삽입할 구분자.
- **`close`**: 반복문 종료 후 추가할 문자열.


### 예제 1: `IN` 절에서 사용


```xml
<select id="selectByIds" parameterType="list" resultType="MyData">
    SELECT * FROM my_table
    WHERE id IN 
    <foreach collection="list" item="id" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>
```


#### Java 코드

```java
List<Integer> ids = Arrays.asList(1, 2, 3, 4);
List<MyData> results = sqlSession.selectList("namespace.selectByIds", ids);
```


#### 생성되는 SQL

```sql
SELECT * FROM my_table
WHERE id IN (1, 2, 3, 4);
```

### 예제 2: 다중 컬럼 삽입

```xml
<insert id="insertBatch" parameterType="list">
    INSERT INTO my_table (column1, column2)
    VALUES 
    <foreach collection="list" item="item" separator=",">
        (#{item.column1}, #{item.column2})
    </foreach>
</insert>
```

#### Java 코드
```java
List<Map<String, Object>> dataList = new ArrayList<>();
dataList.add(Map.of("column1", "value1", "column2", "value2"));
dataList.add(Map.of("column1", "value3", "column2", "value4"));

sqlSession.insert("namespace.insertBatch", dataList);
```

#### 생성되는 SQL

```sql
INSERT INTO my_table (column1, column2)
VALUES ('value1', 'value2'), ('value3', 'value4');
```


### 예제 3: `Map`을 사용한 반복


```xml
<select id="selectByConditions" parameterType="map" resultType="MyData">
    SELECT * FROM my_table
    WHERE 1=1
    <if test="conditions != null">
        <foreach collection="conditions" item="value" index="key" open="AND (" separator=" OR " close=")">
            ${key} = #{value}
        </foreach>
    </if>
</select>
```


#### Java 코드

```java
Map<String, Object> conditions = new HashMap<>();
conditions.put("column1", "value1");
conditions.put("column2", "value2");

List<MyData> results = sqlSession.selectList("namespace.selectByConditions", Map.of("conditions", conditions));
```


#### 생성되는 SQL

```sql
SELECT * FROM my_table
WHERE 1=1 AND (column1 = 'value1' OR column2 = 'value2');
```

####  map insert

##### XML Mapper

```xml
<insert id="insertByConditions" parameterType="map">
    INSERT INTO my_table (column1, column2)
    VALUES
    <foreach collection="conditions" item="value" index="key" separator=",">
        (#{key}, #{value})
    </foreach>
</insert>
```

##### Java 코드
```java
Map<String, Object> conditions = new HashMap<>();
conditions.put("value1", "data1");
conditions.put("value2", "data2");

sqlSession.insert("namespace.insertByConditions", Map.of("conditions", conditions));
```

##### 생성되는 SQL
```sql
INSERT INTO my_table (column1, column2)
VALUES ('value1', 'data1'), ('value2', 'data2');
```


### 예제 4: `index`를 활용한 반복

```xml
<update id="updateBatch" parameterType="list">
    <foreach collection="list" item="item" index="index" separator=";">
        UPDATE my_table
        SET column1 = #{item.column1}, column2 = #{item.column2}
        WHERE id = #{item.id}
    </foreach>
</update>
```

#### Java 코드

```java
List<Map<String, Object>> updateData = new ArrayList<>();
updateData.add(Map.of("id", 1, "column1", "newValue1", "column2", "newValue2"));
updateData.add(Map.of("id", 2, "column1", "newValue3", "column2", "newValue4"));

sqlSession.update("namespace.updateBatch", updateData);
```

#### 생성되는 SQL

```sql
UPDATE my_table
SET column1 = 'newValue1', column2 = 'newValue2'
WHERE id = 1;
UPDATE my_table
SET column1 = 'newValue3', column2 = 'newValue4'
WHERE id = 2;
```