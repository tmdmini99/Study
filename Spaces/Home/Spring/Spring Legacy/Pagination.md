

## Oracle
Oracle 12c 이상에서 페이징 처리


```sql
SELECT * FROM orders
ORDER BY order_date DESC
OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;

```


이 쿼리는 다음과 같은 의미를 가집니다:

- **`OFFSET 10 ROWS`**: 앞의 10개 행을 건너뜁니다.
- **`FETCH NEXT 10 ROWS ONLY`**: 그 다음 10개의 행을 반환합니다.

#### `OFFSET`과 `FETCH`를 사용한 페이징 쿼리 예시

만약 페이지 번호와 페이지당 레코드 수를 기준으로 페이징 처리를 하려면, 다음과 같이 사용할 수 있습니다:


```sql
SELECT * FROM orders
ORDER BY order_date DESC
OFFSET (:page - 1) * :pageSize ROWS FETCH NEXT :pageSize ROWS ONLY;

```

여기서:

- `:page`: 현재 페이지 번호.
- `:pageSize`: 한 페이지에 보여줄 행의 수.

#### 예: 2페이지를 요청하고, 페이지당 10개의 데이터를 표시하려면


```sql
SELECT * FROM orders
ORDER BY order_date DESC
OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;

```


### Oracle 12c 이전 버전에서 페이징 처리 (ROWNUM 방식)

Oracle 12c 이전 버전에서는 `ROWNUM`을 사용하여 페이징을 구현해야 합니다.

#### 기본 쿼리 구조 (ROWNUM 사용)

Oracle 12c 이전 버전에서는 서브쿼리를 사용해서 `ROWNUM`을 적용합니다.


```sql
SELECT * FROM (
    SELECT orders.*, ROWNUM rnum
    FROM orders
    WHERE ROWNUM <= :page * :pageSize
    ORDER BY order_date DESC
)
WHERE rnum > (:page - 1) * :pageSize;

```


이 쿼리는 특정 범위의 데이터를 조회하여 페이징을 적용합니다:

- `ROWNUM <= :page * :pageSize`: 원하는 페이지의 마지막 행까지 선택합니다.
- `rnum > (:page - 1) * :pageSize`: 원하는 페이지의 시작 행을 설정합니다.

### Oracle 페이징 쿼리와 Spring 프로젝트 연동

Spring 프로젝트에서 페이징 처리를 할 때, 주로 `JdbcTemplate`, `MyBatis`, 또는 `JPA` 등을 사용할 수 있습니다. 각 방법에 맞는 쿼리 작성 및 페이징 처리 방식을 살펴보겠습니다.


JdbcTemplate
```java
int page = 2;  // 현재 페이지
int pageSize = 10;  // 페이지당 데이터 개수
int offset = (page - 1) * pageSize;

String query = "SELECT * FROM orders ORDER BY order_date DESC OFFSET ? ROWS FETCH NEXT ? ROWS ONLY";
List<Order> orders = jdbcTemplate.query(query, new Object[]{offset, pageSize}, new OrderRowMapper());

```


MyBatis
```xml
<select id="selectOrdersWithPagination" resultType="Order">
    SELECT * FROM orders
    ORDER BY order_date DESC
    OFFSET #{offset} ROWS FETCH NEXT #{pageSize} ROWS ONLY
</select>

```


java

```java
int page = 2;
int pageSize = 10;
int offset = (page - 1) * pageSize;

Map<String, Object> params = new HashMap<>();
params.put("offset", offset);
params.put("pageSize", pageSize);

List<Order> orders = sqlSession.selectList("selectOrdersWithPagination", params);

```


Spring Data JPA


Repository 정의:
```java
public interface OrderRepository extends JpaRepository<Order, Long> {
}

```

서비스에서 페이징 요청:
```java
PageRequest pageRequest = PageRequest.of(page, pageSize, Sort.by("orderDate").descending());
Page<Order> ordersPage = orderRepository.findAll(pageRequest);

List<Order> orders = ordersPage.getContent();

```


### 전체 레코드 수 구하기

Oracle에서 페이징 시 전체 레코드 수도 조회해야 전체 페이지 수를 계산할 수 있습니다. 이를 위해 아래와 같이 `COUNT(*)`를 사용합니다:

```sql
SELECT COUNT(*) FROM orders;
```

이를 통해 전체 레코드 수를 구하고, 그 결과를 통해 총 페이지 수를 계산할 수 있습니다.


```java
int totalRecords = jdbcTemplate.queryForObject("SELECT COUNT(*) FROM orders", Integer.class);
int totalPages = (int) Math.ceil((double) totalRecords / pageSize);

```


## Postgre



### PostgreSQL 페이징 쿼리

PostgreSQL에서도 페이징 처리를 위해 `LIMIT`과 `OFFSET`을 사용합니다.

#### 기본적인 쿼리 예시:



```sql
SELECT * FROM orders
ORDER BY order_date DESC
LIMIT 10 OFFSET 20;
```



이 쿼리는 다음과 같은 의미를 가집니다:

- `LIMIT 10`: 한 번에 10개의 행을 가져옴.
- `OFFSET 20`: 앞에서 20개의 행을 건너뜀.

### 페이징을 위한 계산

- **`page`**: 현재 페이지 번호.
- **`pageSize`**: 페이지당 보여줄 행의 수.

#### `OFFSET` 계산 방법:


```text
OFFSET = (page - 1) * pageSize
```


예시: 2페이지를 요청하고, 페이지당 10개의 데이터를 표시하고 싶다면:

```sql
SELECT * FROM orders
ORDER BY order_date DESC
LIMIT 10 OFFSET 10;
```


위 쿼리는 2번째 페이지를 의미하며, 첫 번째 페이지는 건너뛰고 10개의 데이터를 반환합니다.

### PostgreSQL 페이징 쿼리 예시

만약 `page`와 `pageSize`를 변수로 사용할 수 있는 환경이라면, 이를 활용해 페이징 쿼리를 동적으로 작성할 수 있습니다.

#### 예: 2페이지, 페이지당 10개 데이터

```sql
SELECT * FROM orders
ORDER BY order_date DESC
LIMIT 10 OFFSET 10;
```


- `LIMIT 10`: 10개의 레코드를 반환.
- `OFFSET 10`: 첫 번째 페이지의 10개 레코드를 건너뜀.

### 총 레코드 수 구하기

페이징 처리 시, 총 레코드 수를 구하는 것이 필요합니다. 이를 위해 `COUNT(*)`를 사용하여 총 레코드 수를 조회할 수 있습니다:

```sql
SELECT COUNT(*) FROM orders;
```


이 값을 이용해 전체 페이지 수를 계산할 수 있습니다:


```java
int totalRecords = /* COUNT(*) 쿼리로 구한 값 */;
int totalPages = (int) Math.ceil((double) totalRecords / pageSize);
```

### Java 코드 예시 (JDBC 사용)

PostgreSQL에서 Java로 페이징을 구현할 때는 `LIMIT`과 `OFFSET`을 동적으로 설정할 수 있습니다.


```java
int page = 2; // 현재 페이지
int pageSize = 10; // 페이지당 데이터 개수
int offset = (page - 1) * pageSize; // OFFSET 계산

String query = "SELECT * FROM orders ORDER BY order_date DESC LIMIT ? OFFSET ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setInt(1, pageSize); // LIMIT
pstmt.setInt(2, offset);   // OFFSET
ResultSet rs = pstmt.executeQuery();

```


### MyBatis를 사용한 PostgreSQL 페이징 예시

MyBatis를 사용할 경우, SQL 쿼리에서 `LIMIT`과 `OFFSET`을 동적으로 설정할 수 있습니다.

#### MyBatis XML Mapper:


```xml
<select id="selectOrdersWithPagination" resultType="Order">
    SELECT * FROM orders
    ORDER BY order_date DESC
    LIMIT #{pageSize}
    OFFSET #{offset}
</select>
```


Java에서 MyBatis를 사용한 페이징 처리:


```java
int page = 2;
int pageSize = 10;
int offset = (page - 1) * pageSize;

Map<String, Object> params = new HashMap<>();
params.put("pageSize", pageSize);
params.put("offset", offset);

List<Order> orders = sqlSession.selectList("selectOrdersWithPagination", params);
```

### 전체 페이지 수 계산

전체 페이지 수를 계산하려면 총 레코드 수를 먼저 조회하고, 이를 기반으로 전체 페이지 수를 계산합니다:


```sql
SELECT COUNT(*) FROM orders;
```


Java에서 계산:

```java
int totalRecords = // COUNT(*) 결과값;
int totalPages = (int) Math.ceil((double) totalRecords / pageSize);
```


