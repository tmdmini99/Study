### 1. **`PageHelper` 라이브러리 의존성 추가**

먼저, `PageHelper` 라이브러리를 프로젝트에 추가해야 합니다.

#### **Maven 의존성 추가 (pom.xml)**


```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.0</version> <!-- 최신 버전으로 설정 -->
</dependency>
```

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>  
    <settings>  
        <setting name="mapUnderscoreToCamelCase" value="true" />  
        <setting name="jdbcTypeForNull" value="NULL" />  
    </settings>  
    <plugins>  
            <plugin interceptor="com.github.pagehelper.PageInterceptor">  
                <property name="helperDialect" value="postgresql" />  
            </plugin>  
        </plugins>  
</configuration>
```


```xml
<plugins>  
    <plugin interceptor="com.github.pagehelper.PageInterceptor">  
        <property name="helperDialect" value="postgresql" />  
    </plugin>  
</plugins>
```

이런식으로 추가 해줘야함



Gradle 의존성 추가 (build.gradle)

```gradle
implementation 'com.github.pagehelper:pagehelper:5.3.0'  // 최신 버전으로 설정
```


### 2. **`PageHelper` 설정**

`PageHelper`는 기본적으로 SQL 쿼리에서 `LIMIT`과 `OFFSET`을 사용하여 페이징을 처리합니다. 페이징 처리 전에 `PageHelper.startPage(pageNum, pageSize)`를 호출하면 해당 페이지에 필요한 데이터만 가져옵니다.

#### 2.1 **`PageHelper` 사용 예제**


```java
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;

public class MyService {

    private MyMapper myMapper;

    public void getPage(int pageNum, int pageSize) {
        // PageHelper.startPage()로 페이지 번호와 페이지 사이즈 지정
        PageHelper.startPage(pageNum, pageSize);

        // PageHelper를 호출한 후 실행되는 쿼리가 페이징 처리가 됩니다.
        List<MyEntity> list = myMapper.selectAll();

        // PageInfo 객체로 페이지 정보와 데이터를 가져옵니다.
        PageInfo<MyEntity> pageInfo = new PageInfo<>(list);

        // 예시 출력
        System.out.println("Total Pages: " + pageInfo.getPages());
        System.out.println("Total Records: " + pageInfo.getTotal());
        System.out.println("Current Page: " + pageInfo.getPageNum());
        System.out.println("Page Size: " + pageInfo.getPageSize());
        System.out.println("List of Data: " + pageInfo.getList());
    }
}
```

- `PageHelper.startPage(pageNum, pageSize)`는 SQL 쿼리를 실행하기 전에 호출하여 페이징 처리를 시작합니다.
    - `pageNum`은 현재 페이지 번호 (1부터 시작).
    - `pageSize`는 한 페이지에 표시할 데이터 개수.
- `PageInfo`는 페이지에 대한 정보를 담고 있는 객체로, `getList()` 메서드를 통해 실제 데이터 리스트를 얻을 수 있습니다. 또한, 페이지네이션 관련 정보 (총 페이지 수, 총 데이터 수, 현재 페이지 번호 등)를 제공합니다.

---

### 3. **Mapper 인터페이스 예제**


```java
import org.apache.ibatis.annotations.Select;
import java.util.List;

public interface MyMapper {

    @Select("SELECT * FROM my_table")
    List<MyEntity> selectAll();
}
```


- `@Select` 어노테이션을 사용하여 SQL 쿼리를 작성합니다.
- `PageHelper.startPage(pageNum, pageSize)` 호출 후 `myMapper.selectAll()`을 실행하면 자동으로 페이징 처리가 적용됩니다.

---

### 4. **PageInfo 사용 예시**

`PageInfo`는 여러 유용한 메서드를 제공합니다. 예를 들어:

- `getList()` - 실제 데이터 리스트
- `getTotal()` - 전체 데이터 수
- `getPages()` - 전체 페이지 수
- `getPageNum()` - 현재 페이지 번호
- `getPageSize()` - 한 페이지에 표시할 데이터 개수

```java
PageInfo<MyEntity> pageInfo = new PageInfo<>(list);
System.out.println("Total records: " + pageInfo.getTotal());
System.out.println("Total pages: " + pageInfo.getPages());
System.out.println("Current page: " + pageInfo.getPageNum());
System.out.println("Data on current page: " + pageInfo.getList());
```


### 5. **`PageHelper` 설정 (선택 사항)**

`PageHelper`는 다양한 설정을 지원합니다. 예를 들어, SQL을 실행하기 전에 자동으로 페이징 쿼리를 적용하거나, `RowBounds`를 사용할 수 있습니다.

#### 5.1 **MyBatis 설정 파일 (mybatis-config.xml)**


```xml
<configuration>
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageHelper">
            <property name="dialect" value="mysql"/> <!-- 사용하려는 DB에 맞게 설정 -->
            <property name="reasonable" value="true"/>
            <property name="supportMethodsArguments" value="true"/>
        </plugin>
    </plugins>
</configuration>
```


- `dialect`: 사용하는 데이터베이스에 맞게 설정 (MySQL, Oracle, PostgreSQL 등).
- `reasonable`: `true`로 설정하면, 페이지 번호가 0 이하 또는 너무 큰 값일 경우 자동으로 범위 내로 조정합니다.
- `supportMethodsArguments`: `true`로 설정하면 `PageHelper.startPage()`에 메서드 인자를 자동으로 적용합니다.

### 6. **페이징 쿼리 최적화**

`PageHelper`는 기본적으로 `LIMIT`과 `OFFSET`을 사용하여 페이징을 처리합니다. 예를 들어:


```sql
SELECT * FROM my_table LIMIT #{pageSize} OFFSET #{pageNum}
```


이 방식은 간단하고 빠르지만, 데이터 양이 매우 많을 경우 성능에 영향을 줄 수 있습니다. 대용량 데이터 처리에서는 다른 방식의 페이징 처리 전략을 고려할 수 있습니다.




### MVC 패턴에서 `PageHelper` 사용

1. **Model**: 데이터와 관련된 로직을 처리하는 부분 (MyBatis Mapper).
2. **View**: 사용자에게 보여지는 부분 (JSP, Thymeleaf, HTML 등).
3. **Controller**: 사용자 요청을 받고, Model과 View를 연결하는 부분.

`PageHelper`를 **Controller**에서 처리하도록 구현하면 됩니다.

### 1. **Controller에서 페이징 처리**

Controller에서 `PageHelper`를 사용해 페이징을 처리하고, `Model`에 페이징 결과를 담아 **View**로 전달합니다.

#### Controller 클래스

```java
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class ProductController {

    @Autowired
    private ProductMapper productMapper; // ProductMapper를 주입받습니다.

    @RequestMapping("/getProductPage")
    public String getProductPage(int pageNum, int pageSize, Model model) {
        // PageHelper로 페이징 처리 시작
        PageHelper.startPage(pageNum, pageSize);

        // 데이터 가져오기
        List<Product> productList = productMapper.selectAll();

        // PageInfo 객체로 페이지네이션 정보와 리스트 반환
        PageInfo<Product> pageInfo = new PageInfo<>(productList);

        // Model에 페이징 정보와 데이터를 담아서 View로 전달
        model.addAttribute("pageInfo", pageInfo);

        // 페이지를 표시할 View 이름 반환 (예: 'productPage.jsp' 또는 'productPage' (Thymeleaf))
        return "productPage";
    }
}
```


이것도 없어도 됨(java로 mapper 구현했을때만 필요)
```java
@Autowired
    private ProductMapper productMapper; // ProductMapper를 주입받습니다.
```



### 2. **MyBatis Mapper XML**

이제 실제 데이터베이스 쿼리를 XML 파일로 작성합니다. `PageHelper`는 쿼리 결과를 자동으로 페이징 처리해주므로, `PageHelper.startPage()`를 호출한 후에는 MyBatis XML에서 정의된 쿼리를 그대로 실행합니다.

#### `ProductMapper.xml`


```xml
<mapper namespace="com.example.mapper.ProductMapper">

    <!-- 모든 상품 데이터를 가져오는 쿼리 -->
    <select id="selectAll" resultType="com.example.model.Product">
        SELECT * FROM product
    </select>

</mapper>
```


ProductMapper.java

```java
import com.example.model.Product;
import java.util.List;

public interface ProductMapper {

    // 상품 목록을 가져오는 메서드
    List<Product> selectAll();
}
```

### 3. **MyBatis 설정 파일**

`PageHelper`는 MyBatis 설정에서 사용해야 합니다. `PageHelper`의 설정을 `mybatis-config.xml`에 추가해 주세요.

#### `mybatis-config.xml`


```xml
<configuration>
    <!-- MyBatis 설정 -->
    <plugins>
        <!-- PageHelper 플러그인 설정 -->
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <property name="helperDialect" value="mysql"/> <!-- 사용하는 데이터베이스에 맞게 설정 -->
            <property name="reasonable" value="true"/>
            <property name="supportMethodsArguments" value="true"/>
            <property name="params" value="pageNum=pageHelperPageNum,pageSize=pageHelperPageSize"/>
        </plugin>
    </plugins>

    <!-- Mapper 파일 위치 -->
    <mappers>
        <mapper resource="com/example/mapper/ProductMapper.xml"/>
    </mappers>
</configuration>
```


```xml
<!-- Mapper 파일 위치 -->
    <mappers>
        <mapper resource="com/example/mapper/ProductMapper.xml"/>
    </mappers>
```
이거 없어도 됨

### 4. **View에서 페이징 정보 사용 (JSP 또는 Thymeleaf)**

Controller에서 전달한 `PageInfo` 객체를 **View**에서 사용하여 페이지네이션을 구현할 수 있습니다.

#### JSP 예시

```jsp
<!-- productPage.jsp -->
<table>
    <tr>
        <th>Column 1</th>
        <th>Column 2</th>
        <!-- 기타 컬럼 -->
    </tr>
    <c:forEach var="product" items="${pageInfo.list}">
        <tr>
            <td>${product.column1}</td>
            <td>${product.column2}</td>
        </tr>
    </c:forEach>
</table>

<!-- 페이지네이션 링크 -->
<div>
    <c:if test="${pageInfo.pageNum > 1}">
        <a href="/getProductPage?pageNum=1&pageSize=${pageInfo.pageSize}">First</a>
        <a href="/getProductPage?pageNum=${pageInfo.pageNum - 1}&pageSize=${pageInfo.pageSize}">Previous</a>
    </c:if>
    
    <c:forEach var="i" begin="1" end="${pageInfo.pages}">
        <a href="/getProductPage?pageNum=${i}&pageSize=${pageInfo.pageSize}">${i}</a>
    </c:forEach>

    <c:if test="${pageInfo.pageNum < pageInfo.pages}">
        <a href="/getProductPage?pageNum=${pageInfo.pageNum + 1}&pageSize=${pageInfo.pageSize}">Next</a>
        <a href="/getProductPage?pageNum=${pageInfo.pages}&pageSize=${pageInfo.pageSize}">Last</a>
    </c:if>
</div>
```

#### Thymeleaf 예시

```html
<!-- productPage.html -->
<table>
    <tr>
        <th>Column 1</th>
        <th>Column 2</th>
        <!-- 기타 컬럼 -->
    </tr>
    <tr th:each="product : ${pageInfo.list}">
        <td th:text="${product.column1}"></td>
        <td th:text="${product.column2}"></td>
    </tr>
</table>

<!-- 페이지네이션 링크 -->
<div>
    <a th:if="${pageInfo.pageNum > 1}" th:href="@{/getProductPage(pageNum=1, pageSize=${pageInfo.pageSize})}">First</a>
    <a th:if="${pageInfo.pageNum > 1}" th:href="@{/getProductPage(pageNum=${pageInfo.pageNum - 1}, pageSize=${pageInfo.pageSize})}">Previous</a>

    <span th:each="i : ${#numbers.sequence(1, pageInfo.pages)}">
        <a th:href="@{/getProductPage(pageNum=${i}, pageSize=${pageInfo.pageSize})}" th:text="${i}"></a>
    </span>

    <a th:if="${pageInfo.pageNum < pageInfo.pages}" th:href="@{/getProductPage(pageNum=${pageInfo.pageNum + 1}, pageSize=${pageInfo.pageSize})}">Next</a>
    <a th:if="${pageInfo.pageNum < pageInfo.pages}" th:href="@{/getProductPage(pageNum=${pageInfo.pages}, pageSize=${pageInfo.pageSize})}">Last</a>
</div>
```

### 5. **정리**

- **Controller**에서 `PageHelper.startPage()`로 페이징을 설정하고, `Mapper`에서 쿼리를 실행하여 데이터를 가져옵니다.
- **Mapper XML**에서 MyBatis 쿼리를 작성하고, `selectAll()` 메서드를 통해 데이터를 가져옵니다.
- `PageHelper`는 쿼리 결과를 자동으로 페이징 처리합니다.
- `Model`을 통해 `PageInfo` 객체를 View로 전달하고, View에서 이를 사용하여 페이지네이션을 구현합니다.

이 방법으로 **MyBatis XML**을 사용하여 페이지네이션을 구현할 수 있습니다.



controller
```java
package com.kwm.web.board.controller;  
  
  
import com.github.pagehelper.PageHelper;  
import com.github.pagehelper.PageInfo;  
import com.kwm.web.board.service.KWM3300Service;  
import com.kwm.web.board.vo.KWM3300Vo;  
import com.kwm.web.common.vo.BasicParamVo;  
  
  
  
import lombok.RequiredArgsConstructor;  
import lombok.extern.log4j.Log4j2;  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.Model;  
import org.springframework.web.bind.annotation.ModelAttribute;  
import org.springframework.web.bind.annotation.RequestMapping;  
  
import javax.servlet.http.HttpServletRequest;  
import java.util.List;  
  
@Log4j2  
@Controller  
@RequiredArgsConstructor  
@RequestMapping("/board")  
public class KWM3300Controller {  
    private final KWM3300Service service;  
  
  
    @RequestMapping("/3300")  
    public String getKWM3300List(@ModelAttribute("BasicParamVo")BasicParamVo paramVo, Model model){  
        log.info("KWM3300 ::: " + paramVo);  
        PageHelper.startPage(1, paramVo.getSc_PAGE_SIZE());  
        List<KWM3300Vo> list = (List<KWM3300Vo>) service.selectList(paramVo);  
        PageInfo<KWM3300Vo> pageInfo = new PageInfo<>(list);  // PageInfo 객체 생성  
        if(paramVo.getSc_ID() == null){  
            paramVo.setSc_ID(list.get(0).getId());  
        }  
        model.addAttribute("list", list);  
        paramVo.setSc_ID((String) getKWM3300Detail(paramVo, model ,null));  
        log.error(paramVo.getSc_ID());  
        model.addAttribute("BasicParamVo", paramVo);  
        model.addAttribute("pageInfo", pageInfo);  // 페이징 정보 추가  
  
        return "/board/KWM3300";  
    }  
    @RequestMapping("/3300Detail")  
    public Object getKWM3300Detail(@ModelAttribute("BasicParamVo")BasicParamVo paramVo, Model model, HttpServletRequest request){  
        log.info("KWM3300Detail ::: " + paramVo);  
        KWM3300Vo data = (KWM3300Vo) service.selectOne(paramVo);  
        model.addAttribute("data", data);  
        model.addAttribute("BasicParamVo", paramVo);  
        if (request != null) {  
            String requestedWith = request.getHeader("X-Requested-With");  
            if ("XMLHttpRequest".equals(requestedWith)) {  
                return "/board/KWM3300";  
            }  
        }  
        return paramVo.getSc_ID();  
    }  
}
```

list를 선언하기 전에 `PageHelper.startPage(1, paramVo.getSc_PAGE_SIZE());`를 사용
그 이후 `PageInfo<KWM3300Vo> pageInfo = new PageInfo<>(list);  // PageInfo 객체 생성  `
하고 `model.addAttribute("pageInfo", pageInfo);  // 페이징 정보 추가 ` 



데이터 
```json
PageInfo {
    pageNum = 1, pageSize = 5, size = 3, startRow = 1, endRow = 3, total = 3, pages = 1, list = Page {
        count = true, pageNum = 1, pageSize = 5, startRow = 0, endRow = 5, total = 3, pages = 1, reasonable = false, pageSizeZero = false
    } [KWM3300Vo(id = 3, userId = null, title = 서비스 이용 요금, contents = 서비스 이용 요금은 무료이며, 일부 프리미엄 서비스는 유료로 제공됩니다., regDt = 2024 - 12 - 04 12: 25: 46.177555, uptDt = null, url = null, seq = 1), KWM3300Vo(id = 2, userId = null, title = 비밀번호를 잊어버렸어요, contents = 비밀번호를 잊으셨다면, "비밀번호 찾기"
        기능을 이용하여 이메일로 재설정 링크를 받으세요., regDt = 2024 - 12 - 04 12: 25: 44.593955, uptDt = null, url = null, seq = 2), KWM3300Vo(id = 1, userId = null, title = 회원가입 방법, contents = 회원가입은 홈페이지 상단의 "회원가입"
        버튼을 클릭하여 필요한 정보를 입력하면 완료됩니다., regDt = 2024 - 12 - 04 12: 25: 38.571014, uptDt = null, url = null, seq = 3)], prePage = 0, nextPage = 0, isFirstPage = true, isLastPage = true, hasPreviousPage = false, hasNextPage = false, navigatePages = 8, navigateFirstPage = 1, navigateLastPage = 1, navigatepageNums = [1]
}
```