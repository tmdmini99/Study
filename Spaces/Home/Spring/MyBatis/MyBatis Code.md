
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
```java
<insert id="insert" parameterType="map">
    INSERT INTO board_notice (user_id, title, contents, category_id, url, file_id)
    VALUES (#{user_id}, #{title}, #{contents}, #{category_id}, #{url}, #{file_id})
</insert>
```


```java

```


```java

```