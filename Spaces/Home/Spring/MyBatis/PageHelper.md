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