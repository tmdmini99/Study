

 
 `RETURNING`과 MyBatis 사용 (자동 증가 ID 가져오기)

```xml
<insert id="insertArtist" parameterType="map" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO artists (name, genre)
    VALUES (#{name}, #{genre})
    RETURNING id;
</insert>
```

Java 코드:

```java
public int insertAndGetId(Map<String, Object> paramMap) {
    sqlSession.insert("insertArtist", paramMap);
    return (int) paramMap.get("id");
}
```



service
```java
@Service
public class OrderService {
    @Autowired
    private OrderDao orderDao;

    public void insertOrder(Map<String, Object> paramMap) {
        orderDao.insertModal(paramMap);
        // DAO 호출 후 바로 ID를 사용할 수 있음
        System.out.println("Generated ID: " + paramMap.get("id"));
    }
}
```

dao
```java
@Repository
public class OrderDao {
    @Autowired
    private SqlSessionTemplate sqlSession;

    public void insertModal(Map<String, Object> paramMap) {
        sqlSession.insert("OrderMapper.insertModal", paramMap);
    }
}
```

xml
```xml
<insert id="insertModal" parameterType="map" useGeneratedKeys="true" keyProperty="id">
    INSERT INTO orders (customer_name, total_amount)
    VALUES (#{customer_name}, #{total_amount})
    RETURNING id;
</insert>
```

`Service`에서 `dao.insertModal(paramMap)` 호출 후, **동일한 `paramMap` 객체**를 사용하기 때문에 바로 `id`를 `get`할 수 있습니다.

- **Java에서 `Map`은 참조형 데이터 타입**입니다.
- `paramMap`을 메서드에 전달할 때, **객체의 참조 주소**가 전달되므로, 같은 `Map`을 공유합니다.
- `MyBatis`가 `RETURNING`으로 ID를 추가하면, **원본 `paramMap`이 수정**됩니다.


## **중요한 주의사항 (동시성 이슈 방지)**

1. **멀티스레드 환경에서는 주의**:
    
    - `paramMap`이 **스레드 안전하지 않음**.
    - `ThreadLocal`이나 **새로운 Map 복사본**을 사용하는 것이 안전함.
2. **명확한 데이터 사용**:
    
    - `paramMap`을 수정하고 다시 사용하는 경우, **데이터 덮어쓰기 위험**이 있습니다.
- 