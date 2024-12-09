
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