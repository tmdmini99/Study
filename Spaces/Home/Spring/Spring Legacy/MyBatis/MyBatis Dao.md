


## MyBatis Annotaion 방식


```java
@Mapper
public interface UserDAO {

    @Insert("INSERT INTO users (username, password) VALUES (#{username}, #{password})")
    void saveUser(User user);

    // 필요한 경우 사용자 정보를 가져오는 메소드도 추가 가능
    // @Select("SELECT * FROM users WHERE username = #{username}")
    // User findByUsername(String username);
}

```



## SqlSession 및 XML Mapper 방식

```java
@Repository
@RequiredArgsConstructor
public class UserDao {

    private final SqlSession sql;

    public int insert(UserVo paramVo) {
        return sql.insert("dbMapper.insert", paramVo);
    }

    public String selectUser() {
        return sql.selectOne("dbMapper.select");
    }

    public UserVo getLogin(UserVo vo) {
        return sql.selectOne("dbMapper.select");
    }
}

```



- **어노테이션 방식**: 작은 프로젝트나 간단한 CRUD 작업에 적합합니다. 코드가 간결하고 빠르게 개발할 수 있지만, 확장성과 유지보수성에서 한계가 있을 수 있습니다.
	    **작고 간단한 프로젝트**: 어노테이션 방식을 사용하여 간편하게 구현할 수 있습니다.
	
- **SqlSession + XML Mapper 방식**: 대규모 프로젝트나 복잡한 쿼리를 다루는 경우에 적합합니다. SQL 쿼리와 비즈니스 로직이 분리되어 유지보수성과 확장성이 뛰어나지만, 초기 설정이 다소 복잡할 수 있습니다.
	- **중대형 프로젝트**: SQL과 비즈니스 로직을 분리하는 것이 유리하므로, `SqlSession`과 XML Mapper 방식을 사용하는 것이 좋습니다.


