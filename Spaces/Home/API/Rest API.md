

REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

#### **REST란** 
1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.


> **CRUD Operation이란**  
> CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로   
> REST에서의 CRUD Operation 동작 예시는 다음과 같다.

> Create : 데이터 생성(POST)  
> Read : 데이터 조회(GET)  
> Update : 데이터 수정(PUT, PATCH)  
> Delete : 데이터 삭제(DELETE)


![[Pasted image 20231226112653.png]]


### **REST 구성 요소**

REST는 다음과 같은 3가지로 구성이 되어있다. 

1. **자원(Resource) : HTTP URI**
2. **자원에 대한 행위(Verb) : HTTP Method**
3. **자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load**

### **REST의 특징**

**Server-Client(서버-클라이언트 구조)**

- 자원이 있는 쪽이 Server, 자원을 요청하는 쪽이 Client가 된다.
    
    - REST Server: API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
    - Client: 사용자 인증이나 context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
    
- 서로 간 의존성이 줄어든다.

**Stateless(무상태)**

- HTTP 프로토콜은 Stateless Protocol이므로 REST 역시 무상태성을 갖는다.
- Client의 context를 Server에 저장하지 않는다.
    
    - 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
    
- Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
    
    - 각 API 서버는 Client의 요청만을 단순 처리한다.
    - 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
    - 물론 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
    - Server의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.
    

**Cacheable(캐시 처리 가능)**

- 웹 표준 HTTP 프로토콜을 그대로 사용하므로 웹에서 사용하는 기존의 인프라를 그대로 활용할 수 있다.
    
    - 즉, HTTP가 가진 가장 강력한 특징 중 하나인 캐싱 기능을 적용할 수 있다.
    - HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
    
- 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
- 캐시 사용을 통해 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률을 향상시킬 수 있다.

**Layered System(계층화)**

- Client는 REST API Server만 호출한다.
- REST Server는 다중 계층으로 구성될 수 있다.
    
    - API Server는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
    - 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.
    
- PROXY, 게이트웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있다.

**Code-On-Demand(optional)**

- Server로부터 스크립트를 받아서 Client에서 실행한다.
- 반드시 충족할 필요는 없다.

**Uniform Interface(인터페이스 일관성)**

- URI로 지정한 Resource에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
    
    - 특정 언어나 기술에 종속되지 않는다.

REST 디자인 원칙

1. **균일한 인터페이스**. 요청이 어디에서 오는지와 무관하게, 동일한 리소스에 대한 모든 API 요청은 동일하게 보여야 합니다. REST API는 사용자의 이름이나 이메일 주소 등의 동일한 데이터 조각이 오직 하나의 URI(Uniform Resource Identifier)에 속함을 보장해야 합니다. 리소스가 너무 클 필요는 없지만, 이는 클라이언트가 필요로 하는 모든 정보를 포함해야 합니다.  
      
    
2. **클라이언트-서버 디커플링**. REST API 디자인에서 클라이언트와 서버 애플리케이션은 서로 간에 완전히 독립적이어야 합니다. 클라이언트 애플리케이션이 알아야 하는 유일한 정보는 요청된 리소스의 URI이며, 이는 다른 방법으로 서버 애플리케이션과 상호작용할 수 없습니다. 이와 유사하게, 서버 애플리케이션은 HTTP를 통해 요청된 데이터에 전달하는 것 말고는 클라이언트 애플리케이션을 수정하지 않아야 합니다.  
      
    
3. **Stateless**. REST API는 stateless입니다. 이는 각 요청에서 이의 처리에 필요한 모든 정보를 포함해야 함을 의미합니다. 즉, REST API는 서버측 세션을 필요로 하지 않습니다. 서버 애플리케이션은 클라이언트 요청과 관련된 데이터를 저장할 수 없습니다.  
  
    
4. **캐싱 가능성**. 가능하면 리소스를 클라이언트 또는 서버측에서 캐싱할 수 있어야 합니다. 또한 서버 응답에는 전달된 리소스에 대해 캐싱이 허용되는지 여부에 대한 정보도 포함되어야 합니다. 이의 목적은 서버측의 확장성 증가와 함께 클라이언트측의 성능 향상을 동시에 얻는 것입니다.  
      
    
5. **계층 구조 아키텍처**. REST API에서는 호출과 응답이 서로 다른 계층을 통과합니다. 경험에 따르면 클라이언트와 서버 애플리케이션이 서로 간에 직접 연결된다고 가정하지 않는 것이 좋습니다. 통신 루프에는 다수의 서로 다른 중개자가 있을 수 있습니다. REST API는 엔드 애플리케이션 또는 중개자와 통신하는지 여부를 클라이언트나 서버가 알 수 없도록 설계되어야 합니다.  
      
    
6. **코드 온디맨드(옵션)**. REST API는 일반적으로 정적 리소스를 전송하지만, 특정한 경우에는 응답에 실행 코드(예: Java 애플릿)를 포함할 수도 있습니다. 이 경우에 코드는 요청 시에만 실행되어야 합니다.

### **REST의 장단점**

장점 

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.

단점 

- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)

---

**REST API란?**

![](https://blog.kakaocdn.net/dn/bnwnOY/btqUTmALH2G/KNTMYZVfN1P9QqtHdGGadk/img.png)

**RESPT API란 REST의 원리를 따르는 API를 의미합니다.**

**하지만 REST API를 올바르게 설계하기 위해서는 지켜야 하는 몇가지 규칙이 있으며 해당 규칙을 알아 보겠습니다.**

### **REST API 설계 예시**

**1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.**

<blockquote data-ke-size="size16" data-ke-style="style3"><b><b><b>Bad Example<span>&nbsp;</span><a href="http://khj93.com/test/">http://khj93.com/Running/</a></b><b><br><b><b><b><span><b><b>Good Example&nbsp;</b></b> </span><a href="http://khj93.com/test/">http://khj93.com/run/</a>&nbsp;</b><b>&nbsp;</b></b></b></b></b></b></blockquote>

**2. 마지막에 슬래시 (/)를 포함하지 않는다.**

<blockquote data-ke-size="size16" data-ke-style="style3"><b><b><b><b>Bad Example<span>&nbsp;</span><a href="http://khj93.com/test/">http://khj93.com/test/</a>&nbsp;</b><b>&nbsp;<br><b><b><b><span><b><b>Good Example&nbsp;</b></b><span>&nbsp;</span></span><a href="http://khj93.com/test/">http://khj93.com/test</a> </b></b></b></b></b></b></b><b><b><b><b><b><b><b></b></b></b></b></b></b></b></blockquote>

**3. 언더바 대신 하이폰을 사용한다.**

<blockquote data-ke-size="size16" data-ke-style="style3"><b><b><b><b>Bad Example<span>&nbsp;</span><a href="http://khj93.com/test/">http://khj93.com/test_blog</a></b><b><br><b><b><b><span><b><b>Good Example&nbsp;</b></b><span>&nbsp;</span></span><a href="http://khj93.com/test/">http://khj93.com/test-blog</a>&nbsp;</b><b>&nbsp;</b></b></b></b></b></b></b></blockquote>

**4. 파일확장자는 URI에 포함하지 않는다.**

<blockquote data-ke-size="size16" data-ke-style="style3"><b><b><b><b>Bad Example<span>&nbsp;</span><a href="http://khj93.com/test/">http://khj93.com/photo.jpg</a>&nbsp;</b><b>&nbsp;<br><b><b><b><span><b><b>Good Example&nbsp;</b></b><span>&nbsp;</span></span><a href="http://khj93.com/test/">http://khj93.com/photo</a>&nbsp;</b><b>&nbsp;</b></b></b></b></b></b></b><b></b></blockquote>

**5. 행위를 포함하지 않는다.**

<blockquote data-ke-style="style3"><b><b><b>Bad Example<span>&nbsp;</span><a href="http://khj93.com/test/">http://khj93.com/delete-post/1</a>&nbsp;</b><b>&nbsp;<br><b><b><b><span><b><b>Good Example&nbsp;</b></b><span>&nbsp;</span></span><a href="http://khj93.com/test/">http://khj93.com/post/1</a>&nbsp;</b><b>&nbsp;</b></b></b></b></b></b></blockquote>



---

**RESTful이란?**

![](https://blog.kakaocdn.net/dn/Prbbr/btqUOBFqxzH/zhpIrKuFxPKI9QvRzmiBe1/img.png)

**RESTFUL이란 REST의 원리를 따르는 시스템을 의미합니다. 하지만 REST를 사용했다 하여 모두가 RESTful 한 것은 아닙니다. REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며**

모든 CRUD 기능을 POST로 처리 하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API는**REST API**의 설계 규칙을 올바르게 지키지 못한 시스템은 REST API를 사용하였지만 RESTful 하지 못한 시스템이라고 할 수 있습니다.




REST API는 _REST(REpresentational State Transfer)_ 아키텍처 스타일의 디자인 원칙을 준수하는 API입니다. 이러한 이유로 REST API를 RESTful API_라고도 합니다.








---
참조 - https://www.ibm.com/kr-ko/topics/rest-apis

https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80

https://velog.io/@seokkitdo/Network-REST%EB%9E%80-REST-API%EB%9E%80-RESTful%EC%9D%B4%EB%9E%80

https://hahahoho5915.tistory.com/54