

 
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


해결 방법: 안전하게 사용하고 싶을 때 (복사본 사용)

```java
public void insertOrder(Map<String, Object> paramMap) {
    Map<String, Object> safeMap = new HashMap<>(paramMap); // 복사본 생성
    orderDao.insertModal(safeMap);
    System.out.println("Generated ID: " + safeMap.get("id"));
}
```

- **같은 `paramMap` 사용 가능**: `DAO`에서 수정한 데이터가 `Service`에서도 즉시 반영됨.
- **주의사항**: **멀티스레드 환경**에서는 **복사본 사용**을 권장.
- **MyBatis의 `RETURNING`** 기능을 사용하면 **자동 증가 ID**를 안전하게 가져올 수 있음.



```xml
<insert id="insertLabelLang" keyProperty="labelId" useGeneratedKeys="true">
	INSERT INTO labels_translates
	(
	label_nm,
	reg_id,
	country_code,
	label_id
	)VALUES
	(
	#{labelsNm},
	#{regId},
	#{countryCode},
	#{labelId} :: numeric
	)
	RETURNING id as label_id
</insert>
```

RETURNING id as label_id  별칭 지정시 keyProperty="labelId"로 변경


```java
public Map<String, Object> performWorker(BasicCUDParamVo paramVo) throws Exception {  
    Map<String, Object> result = new HashMap<>();  
    KWM1201CUDParamVo param = (KWM1201CUDParamVo) paramVo;  
    param.setRegId(LoginUtil.getCurrentUser().getUserId());  
  
    if(Objects.equals(paramVo.getMode(), "I")){  
        param.setTableNm("companies");  
        String companyId = dao.selectCompany(param);  
        param.setTableNm("labels");  
        String labelId = dao.selectLabel(param);  
        if (labelId == null) {  
            dao.insertLabel(param);  
        }  
        if (companyId == null) {  
            dao.insertCompany(param);  
        }  
        log.error(companyId);  
        log.error(labelId);  
        dao.insert(param);  
        param.setTableNm("companies_translates");  
        companyId = dao.selectCompanyLang(param);  
        log.error(companyId);  
        param.setTableNm("labels_translates");  
        labelId = dao.selectLabelLang(param);  
        log.error(labelId);  
        if (labelId == null) {  
            dao.insertLabelLang(param);  
        }  
        if (companyId == null) {  
            dao.insertCompanyLang(param);  
        }  
        dao.insertTranslates(param);  
        dao.insertTag(param);  
        dao.insertTagTranslates(param);  
    }else if(Objects.equals(paramVo.getMode(), "U")){  
        dao.update(param);  
    }else if(Objects.equals(paramVo.getMode(), "D")){  
        dao.delete(param);  
    }  
  
    result.put("success", true);  
    result.put("msg", "처리완료");  
    result.put("mode", paramVo.getMode());  
  
    return result;  
}
```

이렇게 하면 자동으로 매핑되서 가져온 값을 넣지 않아도 됨


여기서 useGeneratedKeys="true" 없을 경우 자동 매핑 x
```xml
<insert id="insertModal" parameterType="map" keyProperty="id" useGeneratedKeys="true">  
    INSERT INTO ${tableNm}  
    (        <foreach collection="columns" item="column" separator=",">  
            ${column}  
        </foreach>  
        ,reg_id  
    )    VALUES    (        <foreach collection="values" item="value" separator=",">  
            #{value}  
        </foreach>  
    )  
    RETURNING id</insert>
```