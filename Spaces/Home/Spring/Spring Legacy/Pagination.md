

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


---

## 내가 만든 Pagination


```java
@GetMapping("customers")  
@PaginationRequired  
public String getCustomer(@ModelAttribute BasicParamVo basicParamVo, Model model) {  
     List<BasicVo> customersList = (List<BasicVo>) customersService.selectList(basicParamVo);  
    System.out.println(basicParamVo.getPageNumber()+"페이지 2");  
     model.addAttribute("list",customersList);  
     return "customers/customers";  
}
```

컨트롤러에서 `@PaginationRequired  `가 붙어 있는 경우에만 
handler에서 실행



```java
package com.kpop.merch.common.interceptor;  
  
import com.kpop.merch.common.dao.CommonDao;  
import com.kpop.merch.common.util.Pagination;  
import com.kpop.merch.common.util.PaginationRequired;  
import com.kpop.merch.common.vo.BasicParamVo;  
import com.kpop.merch.common.vo.BasicVo;  
import lombok.RequiredArgsConstructor;  
import lombok.extern.log4j.Log4j2;  
import lombok.extern.slf4j.Slf4j;  
import org.springframework.web.method.HandlerMethod;  
import org.springframework.web.servlet.HandlerInterceptor;  
import org.springframework.web.servlet.ModelAndView;  
  
import javax.servlet.http.HttpServletRequest;  
import javax.servlet.http.HttpServletResponse;  
import java.util.List;  
  
@Slf4j  
@RequiredArgsConstructor  
public class CustomInterceptor implements HandlerInterceptor {  
  
    private final CommonDao commonDao;  
  
    @Override  
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {  
  
        return true;  
    }  
  
    @Override  
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {  
  
        httpServletRequest.setAttribute("ordersCount", commonDao.ordersCount());  
  
        // PaginationRequired 어노테이션이 붙어 있는 메서드인 경우  
        if (o instanceof HandlerMethod &&  
            ((HandlerMethod) o).hasMethodAnnotation(PaginationRequired.class)) {  
  
            System.out.println("posthandle");  
  
            // BasicParamVo 객체를 가져옵니다.  
            BasicParamVo basicParamVo = (BasicParamVo) modelAndView.getModel().get("basicParamVo");  
            System.out.println(basicParamVo.getPageNumber()+"페이지 11");  
  
            // list에서 totalCount 계산 (customersList의 크기에서 가져온다고 가정)  
            List<? extends BasicVo> list = (List<? extends BasicVo>) modelAndView.getModel().get("list");  
            int totalCount = 0;  
  
            // 리스트가 null이 아니고, 비어 있지 않을 때만 totalCount 계산  
            if (list != null && !list.isEmpty()) {  
                totalCount = list.get(0).getTotalCount() > 0 ? list.get(0).getTotalCount() : 0;  
            } else {  
                System.out.println("List is null or empty");  
            }  
  
            // 페이지 정보 설정  
            int pageSize = basicParamVo.getPageSize();  
            int totalPages = (int) Math.ceil((double) totalCount / pageSize);  // 총 페이지 수 계산  
  
            // 페이지 정보 설정 및 디버깅용 출력  
            basicParamVo.setTotalPages(totalPages);  
  
            // 디버깅용 출력  
            System.out.println("Total Pages: " + basicParamVo.getTotalPages());  
            System.out.println("현재 페이지: " + basicParamVo.getPageNumber());  
  
  
        }  
    }  
  
  
    @Override  
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {  
  
    }}
```



Custom Annotation만들기
```java
package com.kpop.merch.common.util;  
  
import java.lang.annotation.ElementType;  
import java.lang.annotation.Retention;  
import java.lang.annotation.RetentionPolicy;  
import java.lang.annotation.Target;  
  
@Target(ElementType.METHOD) // 메서드에 적용  
@Retention(RetentionPolicy.RUNTIME) // 런타임까지 유지  
public @interface PaginationRequired {  
}
```


Pagination 비동기 처리
```js
function navigatePage(direction, currentPage, totalPages) {  
    // 방향에 따라 페이지 번호를 조정합니다.  
    currentPage += direction;  
  
    // 페이지 번호가 1보다 작거나 총 페이지 수보다 크면 수정하지 않음  
    if (currentPage < 1) {  
        currentPage = 1;  
    }  
    if (currentPage > totalPages) {  
        currentPage = totalPages; // 최대 페이지 수 설정  
    }  
    // 페이지 번호가 제대로 설정되었는지 확인  
        console.log("Navigating to page:", currentPage);  
  
    // fetch로 JSP 페이지 요청  
    // 현재 URL 가져오기  
    const currentUrl = new URL(window.location.href);  
  
    // 페이지 번호 파라미터를 URL에 추가 또는 수정합니다.  
    currentUrl.searchParams.set('pageNumber', currentPage);  
    currentUrl.searchParams.set('name', "test");  
    // fetch 요청 시 URL을 확인  
       console.log("Fetching URL:", currentUrl.toString());  
    fetch(currentUrl)  
        .then(response => {  
            if (!response.ok) {  
                throw new Error('Network response was not ok');  
            }  
            return response.text(); // HTML 내용 반환  
        })  
        .then(html => {  
            // <body> 내용을 업데이트합니다.  
            // 서버에서 응답받은 HTML을 결과 영역에 삽입  
             let newDocument = new DOMParser().parseFromString(html, 'text/html');  
             let newContent = newDocument.getElementById('content'); // 업데이트할 부분 선택  
  
             if (newContent) {  
                    document.getElementById('content').innerHTML = newContent.innerHTML; // 기존 content만 업데이트  
  
             }  
        })  
        .catch(error => {  
            console.error('There has been a problem with your fetch operation:', error);  
        });  
}
```




Pagination 앞뒤 버튼
```jsp
<c:if test="${list[0].totalCount > 50}">  
    <div class="Polaris-IndexTable__PaginationWrapper">  
        <nav aria-label="페이지 매김" class="Polaris-Pagination Polaris-Pagination--table">  
            <div class="Polaris-Box" style="--pc-box-background: var(--p-color-bg-surface-secondary); --pc-box-padding-block-start-xs: var(--p-space-150); --pc-box-padding-block-end-xs: var(--p-space-150); --pc-box-padding-inline-start-xs: var(--p-space-300); --pc-box-padding-inline-end-xs: var(--p-space-200);">  
                <div class="Polaris-InlineStack" style="--pc-inline-stack-align: center; --pc-inline-stack-block-align: center; --pc-inline-stack-wrap: wrap; --pc-inline-stack-flex-direction-xs: row;">  
                    <div class="Polaris-Pagination__TablePaginationActions" data-buttongroup-variant="segmented">  
                        <div>  
                            <!-- 이전 페이지 버튼 -->  
                            <button id="previousURL" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly" aria-label="이전" type="button" onclick="navigatePage(-1, ${basicParamVo.pageNumber}, ${basicParamVo.totalPages})" ${basicParamVo.pageNumber==1 ? 'disabled' : '' }>  
                                <span class="Polaris-Button__Icon">  
                                    <span class="Polaris-Icon">  
                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">  
                                            <path fill-rule="evenodd" d="M9.78 3.47a.75.75 0 0 1 0 1.06l-3.47 3.47 3.47 3.47a.749.749 0 1 1-1.06 1.06l-4-4a.75.75 0 0 1 0-1.06l4-4a.75.75 0 0 1 1.06 0"></path>  
                                        </svg>  
                                    </span>  
                                </span>  
                            </button>  
                        </div>  
                        <div>  
                            <!-- 다음 페이지 버튼 -->  
                            <button id="nextURL" class="Polaris-Button Polaris-Button--pressable Polaris-Button--variantSecondary Polaris-Button--sizeMedium Polaris-Button--textAlignCenter Polaris-Button--iconOnly" aria-label="다음" type="button" onclick="navigatePage(1, ${basicParamVo.pageNumber}, ${basicParamVo.totalPages})" ${basicParamVo.pageNumber==basicParamVo.totalPages ? 'disabled' : '' }>  
                                <span class="Polaris-Button__Icon">  
                                    <span class="Polaris-Icon">  
                                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16">  
                                            <path fill-rule="evenodd" d="M5.72 12.53a.75.75 0 0 1 0-1.06l3.47-3.47-3.47-3.47a.749.749 0 1 1 1.06-1.06l4 4a.75.75 0 0 1 0 1.06l-4 4a.75.75 0 0 1-1.06 0"></path>  
                                        </svg>  
                                    </span>  
                                </span>  
                            </button>  
                        </div>  
                    </div>  
                </div>  
            </div>  
        </nav>  
    </div>  
</c:if>
```