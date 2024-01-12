
**MyBatis란?**
- Mybatis는 **자바 오브젝트**와 **SQL**사이의 자동 매핑 기능을 지원하는 **ORM(Object relational Mapping)프레임워크** 이다.

> ORM이란 Object Relational Mapping의 줄임말
	객체 지향 프로그래밍과 관계형 데이터베이스간에 통신시 발생하는 **패러다임 불일치 문제를 해결**하고자 객체와 관계형 데이터베이스 간에 데이터를 자동으로 매핑해주는 기술을 의미



>프레임워크란?
>	**목적에 따라 고민할 필요없이 이용할 수 있도록 일괄로 가져 다 쓰도록 만들어 놓은 틀**


- **SQ**L을 별도의 파일로 분리해서 관리하게 해준다.
- Hibernate나 JAP(Java Persistence Api)처럼 새로운 DB프로그래밍 패러다임을 익혀야하는 부담 없이 **SQL을 그대로 이용**하면서 **JDBC코드 작성의 불편함도 제거**해주고 **도메인 객체나 VO객체를 중심으로 개발이 가능하다는 장점**이 있다.

객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 좀 더 쉽게 할 수 있게 도와 주는 개발 프레임 워크로서 JDBC를 통해 데이터베이스에 엑세스하는 작업을 캡슐화하고 일반 SQL 쿼리, 저장 프로 시저 및 고급 매핑을 지원하며 모든 JDBC 코드 및 매개 변수의 중복작업을 제거 합니다. Mybatis에서는 프로그램에 있는 SQL쿼리들을 한 구성파일에 구성하여 프로그램 코드와 SQL을 분리할 수 있는 장점을 가지고 있습니다.



![[MyBatis4.png]]


- SQL Mapper - 객체와 SQL문을 매핑
- 내부에 JDBC 사용
- SQL로 직접 DB 데이터 다룸
- 복잡한 쿼리 짤 때 좋음
- 간단한 CRUD 쿼리도 모두 작성해야 하는 단점 존재





**MyBatis 특징**

복잡한 쿼리나 다이나믹한 쿼리에 강하다 - 반대로 비슷한 쿼리는 남발하게 되는 단점이 있다.

프로그램 코드와 SQL 쿼리의 분리로 코드의 간결성 및 유지보수성 향상

resultType, resultClass등 Vo를 사용하지 않고 조회결과를 사용자 정의 DTO, MAP 등으로 맵핑하여 사용 할 수 있다.

빠른 개발이 가능하여 생산성이 향상된다.

**MyBatis3 구조**

**MyBatis3의 Databases Access 구조**

![[MyBatis1.jpg]]

**MyBatis3의 컴포넌트 구조**

**MyBatis3 주요 구성 요소**

|   |   |
|---|---|
|구성 요소 / 구성 파일|설명|
|MyBatis configuration file|MyBatis3의 작업 설정을 설명하는 XML 파일입니다.  <br> 데이터베이스의 연결 대상, 매핑 파일의 경로, MyBatis3의 작업 설정 등과 같은 세부 사항을 설명하는 파일입니다. Spring과 통합하여 사용할 때 데이터베이스의 연결 대상과 매핑 파일 경로 설정을 구성 파일에 지정할 필요가 없습니다. 그러나 MyBatis3의 기본 작업을 변경하거나 확장 할 때 설정이 수행됩니다.|
|org.apache.ibatis.session.SqlSessionFactoryBuilder|MyBatis3 구성 파일을 읽고 생성하는 SqlSessionFactory 구성 요소입니다.  <br>이 구성 요소는 스프링과 통합되어 사용할 때 애플리케이션 클래스에서 직접 처리하지 않습니다.|
|org.apache.ibatis.session.SqlSessionFactory|SqlSession을 생성하는 구성 요소입니다.  <br>이 구성 요소는 스프링과 통합되어 사용할 때 애플리케이션 클래스에서 직접 처리하지 않습니다.|
|org.apache.ibatis.session.SqlSession|SQL 실행 및 트랜잭션 제어를 위한 API를 제공하는 구성 요소입니다.  <br>MyBatis3를 사용하여 데이터베이스에 액세스할 때 가장 중요한 역할을 하는 구성 요소입니다.  <br>이 구성 요소를 스프링과 통합하여 사용할 경우 애플리케이션 클래스에서 직접 처리하지 않습니다.|
|Mapper interface|typeafe에서 매핑 파일에 정의된 SQL을 호출하는 인터페이스입니다.  <br>MyBatis3는 매퍼 인터페이스에 대한 구현 클래스를 자동으로 생성하므로 개발자는 인터페이스만 생성하면 됩니다.|
|Mapping file|SQL 및 O/R 매핑 설정을 설명하는 XML 파일입니다.|

**MyBatis3 주요 구성 요소가 Database Access 하는 순서**



![[MyBatis2.jpg]]



(1)~(3)은 응용 프로그램 시작시 수행되는 프로세스입니다

|   |   |
|---|---|
|1|응용 프로그램이 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청합니다.|
|2|SqlSessionFactoryBuilder는 SqlSessionFactory를 생성하기 위한 MyBatis 구성 파일을 읽습니다.|
|3|SqlSessionFactoryBuilder는 MyBatis 구성 파일의 정의에 따라 SqlSessionFactory를 생성합니다.|

(4)~(10)은 클라이언트의 각 요청에 대해 수행되는 프로세스입니다.

|   |   |
|---|---|
|4|클라이언트가 응용 프로그램에 대한 프로세스를 요청합니다.|
|5|응용 프로그램은 SqlSessionFactoryBuilder를 사용하여 빌드된 SqlSessionFactory에서 SqlSession을 가져옵니다.|
|6|SqlSessionFactory는 SqlSession을 생성하고 이를 애플리케이션에 반환합니다.|
|7|응용 프로그램이 SqlSession에서 매퍼 인터페이스의 구현 개체를 가져옵니다.|
|8|응용 프로그램이 매퍼 인터페이스 메서드를 호출합니다.|
|9|매퍼 인터페이스의 구현 개체가 SqlSession 메서드를 호출하고 SQL 실행을 요청합니다.|
|10|SqlSession은 매핑 파일에서 실행할 SQL을 가져와 SQL을 실행합니다.|

**MyBatis-Spring의 컴포넌트 구조**

|   |   |
|---|---|
|구성 요소 / 구성 파일|설명|
|org.mybatis.spring.SqlSessionFactoryBean|SqlSessionFactory를 작성하고 Spring DI 컨테이너에 개체를 저장하는 구성 요소.  <br>표준 MyBatis3에서 SqlSessionFactory는 MyBatis 구성 파일에 정의된 정보를 기반으로 합니다. 그러나 SqlSessionFactoryBean을 사용하면 MyBatis 구성 파일이 없어도 SqlSessionFactory를 빌드할 수 있습니다.|
|org.mybatis.spring.mapper.MapperFactoryBean|Singleton Mapper 개체를 만들고 Spring DI 컨테이너에 개체를 저장하는 구성 요소.  <br>MyBatis3 표준 메커니즘에 의해 생성된 매퍼 객체는 스레드가 안전하지 않습니다. 따라서 각 스레드에 대한 인스턴스를 할당해야 했습니다. MyBatis-Spring 구성 요소에 의해 생성된 매퍼 개체는 안전한 매퍼 개체를 생성할 수 있습니다. 따라서 서비스 등 싱글톤 구성요소에 DI를 적용할 수 있습니다.|
|org.mybatis.spring.SqlSessionTemplate|SqlSession 인터페이스를 구현하는 Singleton 버전의 SqlSession 구성 요소.  <br>MyBatis3 표준 메커니즘에 의해 생성된 SqlSession 개체가 스레드에 안전하지 않습니다. 따라서 각 스레드에 대한 인스턴스를 할당해야 했습니다. MyBatis-Spring 구성 요소에서 생성된 SqlSession 개체는 안전한 스레드 SqlSession 개체를 생성할 수 있습니다. 따라서 서비스 등 싱글톤 구성요소에 DI를 적용할 수 있습니다.|



![[MyBatis3.jpg]]




(1)~(4)은 응용 프로그램 시작시 수행되는 프로세스입니다

|   |   |
|---|---|
|1|SqlSessionFactoryBean은 SqlSessionFactoryBuilder를 위해 SqlSessionFactory를 빌드하도록 요청합니다.|
|2|SessionFactoryBuilder는 SqlSessionFactory 생성을 위해 MyBatis 구성 파일을 읽습니다.|
|3|SqlSessionFactoryBuilder는 MyBatis 구성 파일의 정의에 따라 SqlSessionFactory를 생성합니다. 따라서 생성된 SqlSessionFactory는 Spring DI 컨테이너에 의해 저장됩니다.|
|4|MapperFactoryBean은 안전한 SqlSession(SqlSessionTemplate) 및 스레드 안전 매퍼 개체(Mapper 인터페이스의 프록시 객체)를 생성합니  다. 따라서 생성되는 매퍼 객체는 스프링 DI 컨테이너에 의해 저장되며 서비스 클래스 등에 DI가 적용됩니다. 매퍼 개체는 안전한 SqlSession(SqlSessionTemplate)을 사용하여 스레드 안전 구현을 제공합니다.|

(5)~(11)은 클라이언트의 각 요청에 대해 수행되는 프로세스입니다.

|   |   |
|---|---|
|5|클라이언트가 응용 프로그램에 대한 프로세스를 요청합니다.|
|6|애플리케이션(서비스)은 DI 컨테이너에서 주입한 매퍼 개체(매퍼 인터페이스를 구현하는 프록시 개체)의 방법을 호출합니다.|
|7|매퍼 객체는 호출된 메소드에 해당하는 SqlSession (SqlSessionTemplate ) 메서드를 호출합니다.|
|8|SqlSession (SqlSessionTemplate )은 프록시 사용 및 안전한 SqlSession 메서드를 호출합니다.|
|9|프록시 사용 및 스레드 안전 SqlSession은 트랜잭션에 할당된 MyBatis3 표준 SqlSession을 사용합니다.  <br> 트랜잭션에 할당된 SqlSession이 존재하지 않는 경우 SqlSessionFactory 메서드를 호출하여 표준 MyBatis3의 SqlSession을 가져옵니다.|
|10|SqlSessionFactory는 MyBatis3 표준 SqlSession을 반환합니다.  <br> 반환된 MyBatis3 표준 SqlSession이 트랜잭션에 할당되기 때문에 동일한 트랜잭션 내에 있는 경우 새 SqlSession을 생성하지 않고 동일한 SqlSession을 사용합니다.on 메서드를 호출하고 SQL 실행을 요청합니다.|
|11|MyBatis3 표준 SqlSession은 매핑 파일에서 실행할 SQL을 가져와 실행합니다.|






---
참조 - https://khj93.tistory.com/entry/MyBatis-MyBatis%EB%9E%80-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%A0%95%EB%A6%AC


https://velog.io/@kjenotn/Spring-MyBatis-%EA%B0%9C%EB%85%90-%ED%8A%B9%EC%A7%95