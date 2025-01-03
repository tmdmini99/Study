

`@RestController`를 사용하면, `@Controller`와 `@ResponseBody`를 결합한 것과 같은 역할을 합니다. 즉, `@RestController`는 해당 클래스에 있는 모든 메서드가 **기본적으로 JSON 형식의 데이터를 반환**하도록 동작합니다.

### `@RestController`를 사용하는 경우

- **API에서 JSON이나 XML 데이터를 반환할 때**: AJAX 요청을 처리하여 데이터를 반환하거나 RESTful API를 만들 때 적합합니다.
- 모든 메서드가 데이터를 반환하고, 템플릿(JSP 등)을 렌더링할 필요가 없을 때 사용합니다.

```java
@RestController
@RequestMapping("/customers/*")
@RequiredArgsConstructor
public class CustomersController {

    private final CustomersService customersService;

    // JSON 데이터를 반환하는 메서드
    @GetMapping("customer-info")
    public CustomerVo getCustomerInfo(@RequestParam("id") String customerId) {
        return customersService.getCustomerInfo(customerId); // JSON 형식으로 반환
    }
}

```


방식으로 `@RestController`를 사용하면 모든 메서드가 **자동으로 JSON 형식**으로 데이터를 반환하게 됩니다. 따라서 별도로 `@ResponseBody`를 메서드마다 붙일 필요가 없습니다.

### `@RestController`가 적합하지 않은 경우

- **템플릿 파일(JSP, Thymeleaf 등)을 렌더링해야 할 때**: `@RestController`는 JSP 파일이나 HTML 파일을 반환하지 않습니다. 템플릿 렌더링이 필요하면 `@Controller`를 사용해야 합니다.

예를 들어, 아래처럼 JSP 파일을 반환하려고 하면, `@RestController`는 텍스트 데이터를 그대로 반환하기 때문에 의도한 동작이 이루어지지 않습니다.



```java
@RestController
@RequestMapping("/customers/*")
@RequiredArgsConstructor
public class CustomersController {

    private final CustomersService customersService;

    // 이 메서드는 JSP 파일을 반환해야 하지만 RestController에서는 동작하지 않음
    @GetMapping("customer")
    public String ordersList(@ModelAttribute BasicParamVo basicParamVo, Model model) {
        List<OrdersVo> list = customersService.selectList(basicParamVo);
        model.addAttribute("list", list);
        return "customers/customers"; // JSP 페이지를 반환하고 싶지만 RestController에서는 작동하지 않음
    }
}

```


- **JSON 데이터만 반환해야 하는 경우**라면 `@RestController`가 편리하고 적합합니다.
- **JSP 같은 템플릿을 렌더링해야 하는 경우**에는 `@Controller`를 사용하고, JSON 응답이 필요한 메서드에만 `@ResponseBody`를 사용해야 합니다.

따라서 **JSP와 JSON 응답을 둘 다 사용하려면** `@Controller`를 사용하고, 특정 메서드에만 `@ResponseBody`를 적용하는 방식이 더 적합합니다.




```java
@GetMapping("orders/{id}")
public String ordersList(@PathVariable("id") Long id, Model model) {
    // 여기서 id는 URL에서 가져온 값입니다.
    List<OrdersVo> list = ordersService.selectList(id);
    model.addAttribute("list", list);
    return "orders/orders";
}

```


 `@GetMapping("orders/{id}")`는 URL 경로에서 동적으로 값을 받을 수 있도록 설정한 것입니다. 이 구조에서 `{id}`는 URL의 경로 변수로, 클라이언트가 요청하는 URL의 특정 부분에 해당하는 값을 자동으로 메서드의 파라미터로 매핑합니다.


`@PathVariable`은 Spring MVC에서 URL 경로의 일부를 메서드 파라미터로 전달할 수 있게 해주는 애너테이션입니다. 이 애너테이션을 사용하면 클라이언트가 보낸 요청의 URL에서 특정 값을 추출하여 사용할 수 있습니다.


- `@PathVariable("id") Long id`: `@PathVariable` 애너테이션을 사용하여 URL의 `{id}` 부분을 `id`라는 이름의 메서드 파라미터로 받습니다. 이 값은 URL 요청에 따라 다르게 들어오며, 이 메서드에서는 주문 ID를 나타냅니다.
- 예를 들어, `GET /orders/123` 요청이 들어오면 `id`는 `123`이 되고, 이 값을 사용하여 주문 목록을 조회합니다.




`@RequestMapping("/orders/*")`와 `@RequestMapping("/orders")`는 비슷한 점도 있지만, 주요한 차이점이 있습니다. 이 두 매핑은 요청을 처리하는 방식이 다릅니다. 아래에서 각각의 의미와 차이점을 설명하겠습니다.

### 1. `@RequestMapping("/orders/*")`

- **의미**: 이 매핑은 `/orders`로 시작하는 모든 경로에 대해 해당 컨트롤러의 메서드를 처리합니다.
- **예시**:
    - `/orders` (리스트 페이지)
    - `/orders/123` (주문 상세 페이지)
    - `/orders/abc` (다른 주문 상세 페이지)
- **장점**: 한 컨트롤러에서 `/orders`로 시작하는 모든 요청을 쉽게 처리할 수 있습니다.
- **단점**: 모든 경로를 처리하는 경향이 있어, 특정 경로에 대해 세부적인 처리가 필요할 경우 관리가 복잡해질 수 있습니다.

### 2. `@RequestMapping("/orders")`

- **의미**: 이 매핑은 `/orders` 경로만 처리합니다. 그 아래에 세부적인 메서드에서 추가 매핑을 설정해야 합니다.
- **예시**:
    - `/orders` (리스트 페이지) → `@GetMapping("")` 또는 `@GetMapping("/")`
    - `/orders/{id}` (주문 상세 페이지) → `@GetMapping("/{id}")`
- **장점**: 더 명확하고 유지 관리하기 쉽습니다. 각 요청에 대해 구체적으로 매핑할 수 있어, 나중에 수정하기도 용이합니다.
- **단점**: 한 컨트롤러가 처리하는 경로가 많아질 경우, 코드가 길어질 수 있습니다.


여기에서 `@GetMapping("orders/{id}")`는 `/orders/orders/{id}`로 매핑되지 않습니다. `@RequestMapping("/orders/*")` 때문에 `/orders/orders/{id}` 경로가 제대로 매핑되지 않을 수 있습니다.

이럴 경우 /orders 로 설정






이런식으로 object로 선언하여 return을 여러가지로 줄수 있음
```java
package com.kwm.web.board.controller;  
  
import com.kwm.web.board.service.KWM3100Service;  
  
import com.kwm.web.board.vo.KWM3100Vo;  
import com.kwm.web.board.vo.KWM3100Vo;  
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
public class KWM3100Controller {  
    private final KWM3100Service service;  
  
    @RequestMapping("/3100")  
    public String getKWM1200List(@ModelAttribute("BasicParamVo")BasicParamVo paramVo, Model model){  
        log.info("KWM3100 ::: " + paramVo);  
        List<KWM3100Vo> list = (List<KWM3100Vo>) service.selectList(paramVo);  
        if(paramVo.getSc_id() == null){  
            paramVo.setSc_id(list.get(0).getId());  
        }  
        KWM3100Vo data = (KWM3100Vo) service.selectOne(paramVo);  
        model.addAttribute("list", list);  
        model.addAttribute("data", list);  
        return "/board/KWM3100";  
    }  
    @RequestMapping("/3100Detail")  
        public Object getKWM3100Detail(@ModelAttribute("BasicParamVo")BasicParamVo paramVo, Model model, HttpServletRequest request){  
            log.info("KWM3100Detail ::: " + paramVo);  
            KWM3100Vo data = (KWM3100Vo) service.selectOne(paramVo);  
            model.addAttribute("data", data);  
            log.error(paramVo.getSc_id());  
            model.addAttribute("BasicParamVo", paramVo);  
            if (request != null) {  
                String requestedWith = request.getHeader("X-Requested-With");  
                if ("XMLHttpRequest".equals(requestedWith)) {  
                    return "/board/KWM3100";  
                }  
            }  
            return paramVo.getSc_id();  
        }  
}
```



`MultipartHttpServletRequest`는 **파일 업로드**를 다루기 위한 스프링에서 제공하는 **인터페이스**입니다. 이 인터페이스는 **HTTP 요청**이 파일을 포함한 **멀티파트(multipart)** 데이터로 전송될 때 사용됩니다. 파일 업로드를 처리하는 데 특화된 메서드를 제공하여, 클라이언트로부터 전송된 파일 데이터를 쉽게 처리할 수 있도록 도와줍니다.

### 멀티파트 요청이란?

웹 애플리케이션에서 파일 업로드 시, 일반적인 HTTP 요청은 `application/x-www-form-urlencoded` 형태로 전송됩니다. 그러나 파일과 같은 바이너리 데이터를 포함한 HTTP 요청은 `multipart/form-data` 형태로 전송됩니다. 이 형식은 파일뿐만 아니라 텍스트 데이터도 함께 전송할 수 있게 해줍니다.

`MultipartHttpServletRequest`는 `HttpServletRequest`의 확장 인터페이스로, **멀티파트 형식으로 전송된 HTTP 요청**을 처리할 수 있도록 도와줍니다. 예를 들어, 파일을 포함한 HTML 폼 데이터를 서버로 전송할 때 이 인터페이스를 사용하여 전송된 파일과 데이터를 쉽게 접근하고 처리할 수 있습니다.

### `MultipartHttpServletRequest`의 주요 메서드

`MultipartHttpServletRequest`는 기본 `HttpServletRequest`를 확장하여 멀티파트 요청을 처리하는 몇 가지 유용한 메서드를 제공합니다. 주요 메서드는 다음과 같습니다:

1. **`getFile(String name)`**:
    
    - 요청에서 특정 **파일**을 가져옵니다.
    - `name`은 `<input type="file" name="name">` 태그에서 설정한 파일 필드의 이름입니다.
    - 반환 타입은 `MultipartFile`로, 실제 파일을 처리할 수 있는 메서드들을 제공합니다.
    
    예:

```java
MultipartFile file = request.getFile("file");
```


**`getFiles(String name)`**:

- 요청에서 **여러 파일**을 가져옵니다. 같은 이름의 파일이 여러 개 전송된 경우, 이를 모두 가져올 수 있습니다.
- 반환 값은 `List<MultipartFile>`입니다.

예:


```java
List<MultipartFile> files = request.getFiles("files");
```



**`getParameter(String name)`**:

- 일반적인 HTTP 요청 파라미터를 가져옵니다. 파일과 텍스트 데이터가 혼합된 멀티파트 요청에서 사용합니다.

예:


```java
String title = request.getParameter("title");
```


**`getParameterMap()`**:

- 요청에 포함된 모든 파라미터를 **맵** 형태로 반환합니다.

예:

```java
Map<String, String[]> parameterMap = request.getParameterMap();
```


### `MultipartHttpServletRequest` 사용 예시

보통 `MultipartHttpServletRequest`는 파일 업로드가 필요한 컨트롤러에서 사용됩니다. 스프링에서는 `@RequestMapping` 또는 `@PostMapping` 등을 사용하여, 요청을 처리할 수 있습니다.


```java
@Controller
public class FileUploadController {

    @PostMapping("/upload")
    public String handleFileUpload(MultipartHttpServletRequest request) {
        // 요청에서 파일 가져오기
        MultipartFile file = request.getFile("file");
        
        // 파일이 존재하는 경우 처리
        if (file != null && !file.isEmpty()) {
            String fileName = file.getOriginalFilename();
            // 파일 저장 로직 처리
            try {
                file.transferTo(new File("/path/to/directory/" + fileName));
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        
        // 기타 파라미터 가져오기
        String title = request.getParameter("title");
        
        // 처리 후 리턴
        return "fileUploadSuccess";
    }
}
```

### `MultipartHttpServletRequest`와 `HttpServletRequest` 차이점

- **`HttpServletRequest`**: 기본적인 HTTP 요청 정보를 담고 있는 인터페이스로, 파일이 포함된 요청을 다룰 수 없습니다.
    
- **`MultipartHttpServletRequest`**: `HttpServletRequest`를 확장한 인터페이스로, 멀티파트 형식의 요청을 처리하는 데 특화된 메서드(`getFile`, `getFiles` 등)를 제공합니다.
    

### 스프링에서 멀티파트 요청 설정

멀티파트 파일 업로드 기능을 사용하려면, **스프링 설정**에서 멀티파트 업로드 기능을 활성화해야 합니다. 이를 위해 `application.properties` 또는 `application.yml` 파일에서 멀티파트 설정을 해야 합니다.

**`application.properties` 예시**:

```properties
# 멀티파트 파일 업로드 설정
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=2MB
spring.servlet.multipart.max-request-size=2MB
```



- **`@ModelAttribute`의 동작**:
    
    - `@ModelAttribute`는 요청 데이터를 Java 객체로 바인딩하는 데 사용됩니다.
    - Spring은 요청 데이터를 처리하기 위해 객체를 생성해야 하며, 기본적으로 이를 위해 클래스의 기본 생성자 또는 유일한 생성자를 사용합니다.
    - 그러나 `Map`은 인터페이스이므로 Spring은 이를 직접 인스턴스화할 수 없습니다.
- **`Map` 타입의 한계**:
    
    - `Map` 타입은 인터페이스로, Spring이 객체를 생성하려면 구체적인 구현 클래스(`HashMap`, `LinkedHashMap` 등)가 필요합니다.
    - `@ModelAttribute`는 기본적으로 `Map`과 같은 타입을 처리하도록 설계되지 않았습니다.



### 해결 방법

#### 1. **`@RequestParam`으로 처리**

- 요청 데이터를 개별적으로 처리할 수 있다면 `@RequestParam`을 사용하세요

```java
@PostMapping("/example")
public String handleRequest(@RequestParam Map<String, Object> map) {
    // map에 요청 데이터가 바인딩됩니다.
    return "success";
}
```


#### **`@RequestBody`로 JSON 요청 처리**

- 요청 데이터가 JSON 형식이라면 `@RequestBody`를 사용하여 `Map`으로 직접 바인딩할 수 있습니다.

```java
@PostMapping("/example")
public String handleRequest(@RequestBody Map<String, Object> map) {
    // map에 JSON 데이터가 바인딩됩니다.
    return "success";
}
```


#### **구체적인 `Map` 구현 사용**

- `@ModelAttribute`를 사용해야 한다면 `Map` 대신 구체적인 구현 클래스(`HashMap`)를 사용하세요

```java
@PostMapping("/example")
public String handleRequest(@ModelAttribute HashMap<String, Object> map) {
    // map에 요청 데이터가 바인딩됩니다.
    return "success";
}
```



|어노테이션|용도|데이터 위치|
|---|---|---|
|`@RequestBody`|HTTP 본문(JSON, XML 등)|요청 본문|
|`@RequestParam`|쿼리 파라미터, 폼 데이터|URL 쿼리 또는 폼 데이터|
|`@ModelAttribute`|객체에 폼 데이터 매핑|폼 데이터|
|`@PathVariable`|URL 경로 변수|URL 경로|
|`HttpServletRequest`|HTTP 요청 객체 직접 사용|요청 전반|
|`@RequestPart`|멀티파트 요청의 특정 파트|멀티파트 데이터|
|`@RequestHeader`|요청 헤더 데이터|요청 헤더|

### 3. **멀티파트와 일반 요청을 동시에 지원**

멀티파트 요청인지 확인한 후 동적으로 처리하도록 구현할 수도 있습니다.


```java
@RequestMapping("/modalCUD")
@ResponseBody
public Map<String, Object> modalCCud(@RequestParam Map<String, Object> map, HttpServletRequest request, HttpServletResponse response) throws Exception {
    Map<String, Object> result = new HashMap<>();

    if (request instanceof MultipartHttpServletRequest) {
        MultipartHttpServletRequest multipartRequest = (MultipartHttpServletRequest) request;
        // 멀티파트 요청 처리
        log.info("Multipart request received.");
    } else {
        log.info("Regular request received.");
    }

    commonService.performWorkerModal(map, result);
    log.error(result);
    return result;
}

```
