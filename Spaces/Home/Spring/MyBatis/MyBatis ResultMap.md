
result map 사용법
```xml
<select id="selectList" resultMap="giftCardsResultMap" parameterType="com.kpop.merch.common.vo.BasicParamVo">
    SELECT *
    FROM GIFT_CARDS
    LEFT JOIN CUSTOMERS
    ON GIFT_CARDS.customer_id = CUSTOMERS.id
    <include refid="common.postfix"/>
</select>

<resultMap id="giftCardsResultMap" type="com.kpop.merch.products.vo.GiftCardsVo">
    <!-- GIFT_CARDS 테이블 매핑 -->
    <id property="id" column="id"/>
    <result property="balance" column="balance"/>
    <result property="createdAt" column="created_at"/>
    <result property="updatedAt" column="updated_at"/>
    <result property="currency" column="currency"/>
    <result property="initialValue" column="initial_value"/>
    <result property="disabledAt" column="disabled_at"/>
    <result property="lineItemId" column="line_item_id"/>
    <result property="apiClientId" column="api_client_id"/>
    <result property="userId" column="user_id"/>
    <result property="customerId" column="customer_id"/>
    <result property="note" column="note"/>
    <result property="expiresOn" column="expires_on"/>
    <result property="templateSuffix" column="template_suffix"/>
    <result property="notify" column="notify"/>
    <result property="lastCharacters" column="last_characters"/>
    <result property="orderId" column="order_id"/>

    <!-- CUSTOMERS 테이블 매핑 (CustomersVo) -->
    <association property="customersVo" javaType="com.kpop.merch.customers.vo.CustomersVo">
        <id property="id" column="customers.id"/> <!-- CUSTOMERS 테이블의 id -->
        <result property="name" column="customers.name"/> <!-- 예시: 고객 이름 -->
        <result property="email" column="customers.email"/> <!-- 예시: 고객 이메일 -->
        <!-- 다른 CUSTOMERS 테이블 컬럼들에 대한 매핑 -->
    </association>
</resultMap>

```


자동 매핑 이용 하는법
```xml
<select id="selectList" resultMap="giftCardsResultMap" parameterType="com.kpop.merch.common.vo.BasicParamVo">  
    SELECT *  
    FROM GIFT_CARDS g    LEFT JOIN CUSTOMERS c    ON g.customer_id = c.id    <include refid="common.postfix"/>  
</select>  
  
<resultMap id="giftCardsResultMap" type="com.kpop.merch.products.vo.GiftCardsVo" autoMapping="true">  
    <!-- GIFT_CARDS 테이블 자동 매핑 -->  
    <id property="id" column="id"/>  
    <!-- CUSTOMERS 테이블 매핑 (CustomersVo) -->    <association property="customersVo" javaType="com.kpop.merch.customers.vo.CustomersVo" autoMapping="true">  
        <id property="id" column="customer_id"/>  
        <!-- 다른 CUSTOMERS 테이블 컬럼들 -->  
    </association>  
</resultMap>
```


### resultMap 재사용 방법


```xml
<resultMap id="customersResultMap" type="com.kpop.merch.customers.vo.CustomersVo" autoMapping="true">
    <id property="id" column="customer_id"/>
    <result property="name" column="name"/>
    <result property="email" column="email"/>
    <!-- 다른 CUSTOMERS 테이블 컬럼들 -->
</resultMap>

```

```xml
<resultMap id="giftCardsResultMap" type="com.kpop.merch.products.vo.GiftCardsVo" autoMapping="true">
    <!-- GIFT_CARDS 테이블 자동 매핑 -->
    <id property="id" column="id"/>
    
    <!-- CUSTOMERS 테이블 매핑 (CustomersVo) -->
    <association property="customersVo" javaType="com.kpop.merch.customers.vo.CustomersVo" resultMap="customersResultMap" />
</resultMap>

```



## Bind

```xml
<select id="selectMember" resultType="com.study.chapter04.SelectMemberDto">
    <bind name="pattern" value="'%' + #{nickname} + '%'" />

    SELECT * FROM member WHERE nickname LIKE #{pattern};
</select>
```


```java
Map<String, Object> params = new HashMap<>();
params.put("userId", 123);

List<ExampleVo> examples = sqlSession.selectList("com.kpop.merch.mapper.ExampleMapper.selectWithBind", params);

```


## 2. Cache

MyBatis에서 캐시는 두 가지 종류가 있습니다: **1차 캐시**와 **2차 캐시**. 1차 캐시는 SqlSession 단위로 관리되고, 2차 캐시는 Mapper 단위로 관리됩니다.

### 2.1 1차 캐시

1차 캐시는 기본적으로 활성화되어 있으며, 같은 `SqlSession` 내에서 동일한 쿼리를 여러 번 호출할 경우, 캐시된 결과를 반환합니다.


```java
ExampleVo example1 = sqlSession.selectOne("com.kpop.merch.mapper.ExampleMapper.selectExample", 1);
ExampleVo example2 = sqlSession.selectOne("com.kpop.merch.mapper.ExampleMapper.selectExample", 1);
// example1과 example2는 같은 객체를 참조합니다.
```

### 2.2 2차 캐시 설정

2차 캐시를 사용하려면 매퍼 XML에 캐시를 설정해야 합니다. 예를 들어, 아래와 같이 설정할 수 있습니다.


```xml
<mapper namespace="com.kpop.merch.mapper.ExampleMapper">
    <cache />
    
    <select id="selectExample" resultType="ExampleVo">
        SELECT * 
        FROM example_table 
        WHERE id = #{id}
    </select>
</mapper>

```



#### 2.2.1 2차 캐시 사용 예시

2차 캐시는 사용하려면 각 Mapper에 대해 `<cache />`를 추가해야 합니다.


```xml
<mapper namespace="com.kpop.merch.mapper.ExampleMapper">
    <cache />
    
    <select id="selectAll" resultType="ExampleVo">
        SELECT * FROM example_table
    </select>
</mapper>

```



### 2.3 Cache 사용 시 주의사항

- **2차 캐시 사용 시, 객체의 동등성(equality)**에 대한 주의가 필요합니다. Java의 `equals`와 `hashCode` 메서드가 잘 정의되어 있어야 합니다.
- **2차 캐시를 사용할 때, MyBatis에 캐시를 저장할 수 있는 라이브러리**가 필요할 수 있습니다. 기본적으로는 MyBatis가 자체 캐시를 지원하지만, Ehcache와 같은 외부 캐시 라이브러리를 사용할 수도 있습니다.

### 2.4 외부 캐시 설정 예시 (Ehcache)

Ehcache를 사용할 경우 `ehcache.xml` 파일에 캐시 설정을 추가해야 합니다.




```xml
<ehcache>
    <cache name="exampleMapper"
           maxEntriesLocalHeap="1000"
           eternal="false"
           timeToLiveSeconds="120"
           memoryStoreEvictionPolicy="LRU">
    </cache>
</ehcache>

```


### MyBatis Configuration

MyBatis의 설정 파일인 `mybatis-config.xml`에 캐시 설정을 추가할 수 있습니다.

```xml
<configuration>
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>
    <typeAliases>
        <typeAlias type="com.kpop.merch.products.vo.ExampleVo" alias="ExampleVo"/>
    </typeAliases>
</configuration>

```


2.5 캐시 사용 예시

```java
// 첫 번째 호출 - DB에서 데이터 가져오기
List<ExampleVo> examples1 = sqlSession.selectList("com.kpop.merch.mapper.ExampleMapper.selectAll");

// 두 번째 호출 - 캐시에서 데이터 가져오기
List<ExampleVo> examples2 = sqlSession.selectList("com.kpop.merch.mapper.ExampleMapper.selectAll");

```
1. **1차 캐시 (Local Cache)**:
    
    - MyBatis의 세션 단위로 작동하며, 한 세션 내에서 조회한 데이터는 같은 세션 내에서 재사용됩니다.
    - 동일한 쿼리에 대해 여러 번 요청하더라도 처음 요청했을 때의 결과를 사용하여 데이터베이스를 다시 조회하지 않습니다.
    - 세션이 닫히면 캐시는 사라집니다.
2. **2차 캐시 (Global Cache)**:
    
    - 매퍼(namespace) 단위로 작동하며, 여러 세션 간에 공유될 수 있습니다.
    - 설정을 통해 활성화해야 하며, 데이터베이스에 대한 불필요한 호출을 줄이는 데 도움을 줍니다.
    - 2차 캐시를 사용하면 이전에 조회한 결과를 저장하고, 같은 요청이 들어왔을 때 캐시된 결과를 반환합니다.

### 캐시 사용의 장점

- **성능 향상**: 자주 조회되는 데이터를 메모리에 저장함으로써 데이터베이스의 부하를 줄이고 응답 속도를 향상시킵니다.
- **DB 호출 최소화**: 동일한 쿼리를 여러 번 호출할 필요가 없어, 네트워크 대역폭을 절약하고 데이터베이스 연결 수를 줄입니다.
- **비용 절감**: 데이터베이스의 부하가 줄어들면 운영 비용이 절감될 수 있습니다.

### 단점 및 주의사항

- **데이터 일관성 문제**: 캐시된 데이터가 데이터베이스의 최신 상태와 일치하지 않을 수 있습니다. 예를 들어, 데이터베이스에 데이터가 추가되거나 수정될 때 캐시를 업데이트하지 않으면 오래된 데이터를 사용할 위험이 있습니다.
- **메모리 사용**: 캐시를 사용하면 메모리를 추가로 사용하게 되므로, 적절한 크기 및 관리 전략이 필요합니다.
- **설정의 복잡성**: 2차 캐시를 사용할 경우, 적절한 캐시 전략을 수립하고, 캐시를 어떻게 관리할 것인지에 대한 추가 설정이 필요할 수 있습니다.







---
출처 - 