

컨트롤러에 모델로  css 파일 보내기 
```java
package com.kpop.merch.common.controller;  
  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.ModelAttribute;  
import javax.servlet.ServletContext;  
  
import java.nio.file.Files;  
import java.nio.file.Paths;  
import java.util.ArrayList;  
import java.util.List;  
  
@Controller  
public class HomeController {  
  
   @Autowired  
   private ServletContext servletContext;  
  
   @ModelAttribute("cssFiles") // 모든 요청에 대해 모델에 cssFiles 추가  
   public List<String> cssFiles() {  
       String realPath  = servletContext.getRealPath("/");  
       String cssDirectoryPath = realPath + "resources/css"; // CSS 디렉토리 경로 설정  
       List<String> cssFiles = new ArrayList<>();  
  
       try {  
           // 디렉토리의 모든 CSS 파일 찾기  
           Files.list(Paths.get(cssDirectoryPath))  
                   .filter(path -> path.toString().endsWith(".css"))  
                   .forEach(path -> cssFiles.add("/resources/css/" + path.getFileName().toString())); // Context Path에 맞게 수정  
       } catch (Exception e) {  
           e.printStackTrace(); // 오류 처리  
       }  
  
       return cssFiles; // CSS 파일 목록 반환  
   }  
  
  
    @GetMapping("/")  
    public String index() {  
        return "home"; // => home.jsp 출력  
    }  
  
}
```

이렇게 사용하는 이유는 webapp/resources/css 안에 있기 때문에 절대 경로로 접속
```java
String realPath  = servletContext.getRealPath("/"); 
```


그 후 jsp에서 for문으로 참조 하면 끝

```jsp
<c:forEach var="cssFile" items="${cssFiles}">  
    <link rel="stylesheet" type="text/css" href="${cssFile}" />  
</c:forEach>
```