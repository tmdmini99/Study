
goole reCAPTCH api 적용하는 방법

[https://www.google.com/recaptcha/about/](https://www.google.com/recaptcha/about/)


![[reCAPTCH1.png]]


1. 위 사이트를 들어가면 구글에서 제공하는 캡차를 본인 사이트에 등록 할수 있는데
2. 아래에 예시로 test라는 이름을 가진 reChaptcha v2버전을 만드는데 도메인은 포트번호나 프로토콜은 인식받지않아 localhost로 본인 컴퓨터에서 사용가능하다

![[reCAPTCH2.png]]


아래에 찍지는 못했는데 '제출' 버튼을 클릭하면 특이사항이 없는한 아래 사진처럼 등록될것인데 사이트 키와 비밀키를 사용하는것이다.


![[reCAPTCH3.png]]

IDE프로그램(eclipse)에서 3개의 파일을 만들건데 spring에서 필요한 Controller, 캡차에게 요청을하고 인식 완료되거나 실패되었을때 값을 처리해주는 POJO Class 그리고 View로 JSP까지 총 3개의 파일을 만들것이다.|  
시크릿키로 기본 UI를 받아온 후 ajax를 이용해 입력받은값을 컨트롤러에게 보낸다

```html
<script src="https://www.google.com/recaptcha/api.js"></script>
<script>
$(function() {
$('#add_member_form').submit(function() {
		var captcha = 1;
		$.ajax({
            url: '/pro/VerifyRecaptcha',
            type: 'post',
            data: {
                recaptcha: $("#g-recaptcha-response").val()
            },
            success: function(data) {
                switch (data) {
                    case 0:
                        console.log("자동 가입 방지 봇 통과");
                        captcha = 0;
                		break;
                    case 1:
                        alert("자동 가입 방지 봇을 확인 한뒤 진행 해 주세요.");
                        break;
                    default:
                        alert("자동 가입 방지 봇을 실행 하던 중 오류가 발생 했습니다. [Error bot Code : " + Number(data) + "]");
                   		break;
                }
            }
        });
		if(captcha != 0) {
			return false;
		} 
});
});
</script>
<body>

 <div class="g-recaptcha" data-sitekey="<!-- 여기가 사이트키 -->6Ld1tsgaAAAAAMN22NYiVxQdxPOwXWbzn-MpAhTP"></div>
<button id = "join_button"type="submit">회원가입</button>
</body>
```


1. ajax를 이용해 입력받은값을 컨트롤러에게 보낸다


3. **VerifyRecaptcha**  
    구글에서 제공하는 url주소로 시크릿 키를 통해 View 에서 하는 행동을 분석해서 답을 boolean타입으로 리턴해주는 클래스
    

```java
package kr.or.ddit;
import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.StringReader;
import java.net.URL;

import javax.json.Json;
import javax.json.JsonObject;
import javax.json.JsonReader;
import javax.net.ssl.HttpsURLConnection;
import javax.servlet.http.HttpServletRequest;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

public class VerifyRecaptcha {
	 public static final String url = "https://www.google.com/recaptcha/api/siteverify";
	    private final static String USER_AGENT = "Mozilla/5.0";
	    private static String secret = ""; //local
	 
	    public static void setSecretKey(String key){
	        secret = key;
	    }
	    
	    public static boolean verify(String gRecaptchaResponse) throws IOException {
	        if (gRecaptchaResponse == null || "".equals(gRecaptchaResponse)) {
	            return false;
	        }
	        
	        try{
	        URL obj = new URL(url);
	        HttpsURLConnection con = (HttpsURLConnection) obj.openConnection();
	 
	        // add reuqest header
	        con.setRequestMethod("POST");
	        con.setRequestProperty("User-Agent", USER_AGENT);
	        con.setRequestProperty("Accept-Language", "en-US,en;q=0.5");
	 
	        String postParams = "secret=" + secret + "&response="
	                + gRecaptchaResponse;
	 
	        // Send post request
	        con.setDoOutput(true);
	        DataOutputStream wr = new DataOutputStream(con.getOutputStream());
	        wr.writeBytes(postParams);
	        wr.flush();
	        wr.close();
	 
	        int responseCode = con.getResponseCode();
	        System.out.println("\nSending 'POST' request to URL : " + url);
	        System.out.println("Post parameters : " + postParams);
	        System.out.println("Response Code : " + responseCode);
	 
	        BufferedReader in = new BufferedReader(new InputStreamReader(
	                con.getInputStream()));
	        String inputLine;
	        StringBuffer response = new StringBuffer();
	 
	        while ((inputLine = in.readLine()) != null) {
	            response.append(inputLine);
	        }
	        in.close();
	 
	        // print result
	        System.out.println(response.toString());
	         
	        //parse JSON response and return 'success' value
	        JsonReader jsonReader = Json.createReader(new StringReader(response.toString()));
	        JsonObject jsonObject = jsonReader.readObject();
	        jsonReader.close();
	         
	        return jsonObject.getBoolean("success");
	        }catch(Exception e){
	            e.printStackTrace();
	            return false;
	        }
	    }

}​
```

 Controller  
그 클래스를 받아서 성공과 실패여부를 다시 클라이언트에게 전달해준다


```java
package kr.or.ddit;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.ResponseBody;

//		Controller에서 캡챠에서 받아오는 정보인식?  
@Controller
@RequestMapping("/pro")
public class abc {
	@ResponseBody
	@RequestMapping(value = "/VerifyRecaptcha", method = RequestMethod.POST)
	public int VerifyRecaptcha(HttpServletRequest request) {
		// 시크릿 키를 캡챠를 받아올수 있는 Class에 보내서 그곳에서 값을 출력한다
	    VerifyRecaptcha.setSecretKey("시크릿키");
	    String gRecaptchaResponse = request.getParameter("recaptcha");
	    try {
	       if(VerifyRecaptcha.verify(gRecaptchaResponse))
	          return 0; // 성공
	       else return 1; // 실패
	    } catch (Exception e) {
	        e.printStackTrace();
	        return -1; //에러
	    }
	}
}​
```

1. 톰캣 브라우저를 실행시 뜨는 화면이다

![[reCAPTCH4.jpg]]

---
참조 -  https://sac4686.tistory.com/24 