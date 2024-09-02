**HttpServletRequest**

패키지 : javax.servlet.http.HttpServletRequest

웹 브라우저의 요청 정보를 저장하고 있는 객체

Header 정보, Parameter, Cookie, URI, URL 등의 정보를 읽어들이는 메소드를 가진 클래스

Body의 Streaem을 읽어들이는 메소드를 가지고 있음

**HttpServletResponse**

패키지 : javax.servlet.http.HttpServletResponse

웹 브라우저의 요청에 대한 응답 정보를 저장하고 있는 객체

Content Type, 응답코드, 응답 메시지등을 담아서 전송


**요청과 응답(HTTP Request/Response Header)**

HTTP 메시지는 보통 **Header + Body**로 이루어지는데, Header에는 클라이언트와 서버가 요청 또는 응답으로 부가적인 정보를 전송할 수 있도록 합니다. 헤더는 일반적인 정보를 담은 일반 헤더(General Header)와 본문에 정보가 담긴 엔티티 헤더(Entity Header)가 있고, 요청(Request), 응답(Response) 헤더로 나뉘어집니다.

쉽게, request는 클라이언트에 의해 전달되어 서버로부터 클라이언트 요청에 의해 동작을 일으키는 것이고, response는 그에 대한 서버의 회신입니다.

**▶ 일반 헤더(General Header)에 관한 메소드**

일반 헤더는 HTTP 통신에 대한 일반적인 정보가 포함됩니다. (Date, Connection, Cache-Control, .. )

Cache-Control : HTTP 1.1 지원

Pragma : HTTP 1.0 지원. "no-cache" 이용

Expire : HTTP 1.0 지원. 응답 결과 만료일 지정

|   |   |   |
|---|---|---|
|**Request 메소드**|**설명**|   |
|request.getHeader(String)|헤더의 이름으로 헤더의 값 반환|   |
|**Response 메소드**|**설명**|   |
|response.setHeader("헤더이름", "값")|캐싱 여부 지정|   |
|response.addDateHeader(String, long)|name 헤더에 date를 추가. date는 1970년 1월 1일 이후 흘러간 시간을 1/1000초 단위|   |
|response.setDateHeader(String, long)|name 헤더의 값을 date로 지정. date는 1970년 1월 1일 이후 흘러간 시간을 1/1000초 단위단위|   |

ex)

request.getHeader("Date"); // Date, Connection, Cache-Control, ..

response.setHeader("Cache-Control", "no-cache"); // Connection, Cache-Control, ..

​

**▶ 엔티티 헤더(Entity Header)에 관한 메소드**

엔티티 헤더는 실제 메시지 또는 전송중인 HTTP 본문에 대한 정보로 컨텐츠 길이, 컨텐츠 언어, 인코딩, 만료 날짜 및 기타 정보가 있습니다.

|   |   |   |
|---|---|---|
|**Request 메소드**|**설명**|   |
|request.getContentType() : String|요청된 컨텐츠 타입(MIME Type) 및 인코딩 방식 반환|   |
|request.getContentLength() : int|요청된 컨텐츠 길이 반환|   |
|**Response 메소드**|**설명**|   |
|response.setContentType(String)|응답 컨텐츠 타입(MIME Type) 및 인코딩 방식 정의|   |
|response.setContentLength(int)|응답 컨텐츠 길이 정의|   |

​

**▶ 요청/응답 헤더(Request, Response)에 관한 메소드**

요청 헤더는 요청한 URL, 메소드(GET, POST, HEAD, ..), 요청 생성에 사용된 브라우저 및 기타 정보를 포함합니다.

응답 헤더는 컨텐츠에 사용된 인코딩, 서버 시스템에서 응답을 생성하는 데 사용되는 서버 소프트웨어 및 기타 정보를 포함합니다.

|                                            |                   |     |
| ------------------------------------------ | ----------------- | --- |
| **Request 메소드**                            | **설명**            |     |
| request.getHeader(String) : String         | 헤더의 이름으로 헤더의 값 반환 |     |
| request.getHeaderNames() : Enumeration\<?> | 모든 헤더 명을 반환       |     |
|request.getMethod() : String|요청 메소드를 반환(GET, POST, ..)|   |
|request.getProtocol() : String|요청 시, 사용 중인 프로토콜(HTTP/1.1)을 반환|   |
|request.getServerPort() : int|요청 서버의 Port 번호를 반환|   |
|request.getRequestURL() : String|프로토콜, 서버이름, 포트번호 모두를 포함한 URL 주소 반환|   |
|request.getRequestURI() : String|프로토콜, 서버이름, 포트번호를 제외한 나머지 주소와 파라미터 반환|   |
|request.getScheme() : String|프로토콜 이름(http, https) 반환|   |
|request.getServerName() : String|서버 이름 반환|   |
|request.getContextPath() : String|컨텍스트 경로|   |
|request.getLocale() : String|지역 정보 반환|   |
|request.getParameterNames() : Enumeration|모든 파라미터명을 반환|   |
|request.getParameterValues(String) : String[]|파라미터의 이름으로 해당 파라미터의 값을 String 배열로 반환|   |
|request.getParameter(String) : String|파라미터의 이름으로 해당 파라미터의 값 반환|   |
|request.getParameterMap(String) : Map|파라미터의 이름으로 해당 파라미터터의 값을 Map으로 반환|   |
|request.getLocalAddr() : String|서버의 IP 주소|   |
|request.getLocalName() : String|서버의 도메인 값 반환|   |
|request.getLocalPort() : int|서버 Port|   |
|request.getRemoteAddr()|클라이언트 IP 주소|   |
|request.getRemoteHost()|클라이언트 HOST 값 반환|   |
|request.getRemotePort()|클라이언트 접속 Port|   |
|request.getCookies() : Cookie[]|요청 정보로부터 쿠키를 반환|   |
|request.getSession() : HttpSession|요청 정보로부터 세션을 반환|   |
|request.getRequestedSessionId() : String|요청 정보로부터 세션 ID 반환|   |
|request.isRequestedSessionIdFormCookie() : boolean|세션 ID가 쿠키로 제공되었는지 여부|   |
|request.isRequestedSessionIdFromURL() : boolean|세션 ID가 URL의 일부로 제공되었는지 여부|   |
|request.isRequestedSessionIdValid() : boolean|세션이 유효한지 여부|   |
|**Response 메소드**|**설명**|   |
|response.setHeader("헤더이름", "값")|해당 헤더 이름과 값으로 헤더 정의|   |
|response.setRequestMethod(String)|응답 HTTP 메소드 정의|   |
|response.addCookie(Cookie)|응답 쿠키 추가|   |

**요청/응답 바디(Body)에 관한 메소드**

GET 방식은 URL에 QueryString의 형태로 Parameter를 붙여서 전송합니다.

POST 방식은 Header 영역에 Content-Type으로 데이터 타입을 명시하고, body 영역에 데이터를 넣어서 보냅니다.

★ 기본 객체 (pageContext, request, session, application)

- pageContext : 하나의 JSP 페이지 내에서 공유될 값 정의

→ JSP 페이지에서만 가능(생성, 사용)

→ pageContext 객체 이용(setAttribute, getAttribute)

- request : 한번의 요청을 처리하는데 사용되는 모든 JSP페이지에서 공유될 값 정의

→ 요청이 들어올 때마다, WAS는 request 객체와 response 객체를 만드는데, Servlet의 service() 메소드가 끝날 때 Request Scope가 사라집니다. 즉, 요청을 받아서 응답하기까지 객체가 유효합니다.

→ request 객체 이용(setAttribute, getAttribute)

- session : 한 사용자와 관련된 정보를 JSP들이 공유하기 위해 사용될 값 정의

→ Session Scope은 session 객체가 만들어져 소멸될 때까지 값을 유지, 여러 개의 요청이 들어와도 남아있음

→ request.getSession() 메소드로 session 객체를 얻을 수 있음

→ session 객체 이용(setAttribute, getAttribute)

- application : 같은 애플리케이션 내에 하나의 웹 어플리케이션에 관련하여 모든 사용자가 공유될 값 정의

→ Application Scope은 하나의 application이 생성되고 소멸될 때까지 값을 유지

→ 하나의 Project를 하나의 Application, 하나의 Server에는 여러 개의 Web Application이 존재할 수 있음

→ request.getServletContext() 메소드로 application 객체를 얻을 수 있음

→ application 객체 이용(setAttribute, getAttribute)

|   |   |   |
|---|---|---|
|**Request 메소드**|**설명**|   |
|request.getInputStream() : ServletIntputStream|요청 바디로부터 바이너리 데이터를 읽어들일 수 있는 ServletInputStream 반환|   |
|request.getReader() : BufferedReader|요청 바디로부터 문자 인코딩에 따라 텍스트를 읽어 들이기 위한 BufferedReader 반환|   |
|request.setAttribute(String, Object)|속성의 이름으로 해당 값을 정의|   |
|request.getAttribute(String) : Object|속성의 이름으로 속성의 값 반환|   |
|request.getAttributeNames() : Enumeration|모든 속성 명을 반환|   |
|request.removeAttribute(String)|속성의 이름으로 해당 속성 삭제|   |
|**Response 메소드**|**설명**|   |
|response.getWriter() : PrintWriter|출력을 위한 출력스트림 객체인 PrintWriter 반환|   |

​

**♪ 모든 Parameter 값 표시**

```
Enumeration params = request.getParameterNames(); while (params.hasMoreElements()) { String name = (String) params.nextElement(); String value = request.getParameter(name); logger.debug(name + "=" + value); }
```

**♪ 모든 Cookie 값 표시**

```
Cookie cookies[] = request.getCookies(); for(int i = 0; i < cookies.length; i++) { String name = cookies[i].getName(); String value = cookies[i].getValue(); logger.debug(name + "=" + value); }
```

**♪ 모든 Attribute 값 표시**

```
Enumeration<String> attrs = request.getAttributeNames(); while (attrs.hasMoreElements()) { String name = (String)attrs.nextElement(); String value = (String)request.getAttribute(name); logger.debug(name + " : " + value); }
```

**♪ 모든 Header 값 표시**

```
Enumeration<String> headers = request.getHeaderNames(); while (headers.hasMoreElements()) { String name = (String) headers.nextElement(); String value = request.getHeader(name); logger.debug(name + "=" + value); }
```

**♪ Request Body로 넘어오는 값 표시**

```
DataInputStream dis = new DataInputStream(request.getInputStream()); String str = null while ((str = dis.readLine()) != null) { logger.debug(new String(str.getBytes("ISO-8859-1"), "utf-8") + "/n"); // euc-kr로 전송된 한글은 깨진다. }
```

**♪ PrintWriter 객체를 이용하여 내용 출력**

```
response.setContentType("text/html;charset=utf-8"); PrintWriter out = response.getWriter(); out.print("<html>"); .. out.print("</html>");
```




---
출처 -  https://blog.naver.com/hj_kim97/222316920222