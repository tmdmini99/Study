# **✅ 2. Spring `messages.properties`를 JavaScript i18next와 함께 사용**

Spring `messages.properties`를 유지하면서도 **한 번만 데이터를 불러와 JavaScript에서 i18next로 관리**할 수 있습니다.


📌 2.1 Spring에서 `messages.properties`를 JSON으로 변환하는 API 만들기


```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Locale;
import java.util.Map;

@RestController
public class MessageController {

    @Autowired
    private MessageSource messageSource;

    @GetMapping("/api/messages")
    public Map<String, Map<String, String>> getMessages() {
        Map<String, Map<String, String>> translations = new HashMap<>();
        String[] languages = {"ko", "en"};
        String[] keys = {"order.price", "order.success"};

        for (String lang : languages) {
            Locale locale = new Locale(lang);
            Map<String, String> messages = new HashMap<>();
            for (String key : keys) {
                messages.put(key, messageSource.getMessage(key, null, locale));
            }
            translations.put(lang, messages);
        }

        return translations;
    }
}
```


📌 **API 응답 예제 (`http://localhost:8080/api/messages`)**


```json
{
  "ko": {
    "order.price": "가격",
    "order.success": "주문이 완료되었습니다."
  },
  "en": {
    "order.price": "Price",
    "order.success": "Order completed successfully"
  }
}
```

📌 2.2 JavaScript에서 i18next와 연동하여 메시지 로드


```js
fetch("/api/messages")  // 서버에서 messages.properties 불러오기
    .then(response => response.json())
    .then(translations => {
        i18next.init({
            lng: "ko", // 기본 언어
            fallbackLng: "en",
            resources: translations // 서버에서 가져온 번역 데이터
        }, function(err, t) {
            updateMessages(); // UI 업데이트
        });
    })
    .catch(error => console.error("Error loading messages:", error));

// UI 업데이트 함수
function updateMessages() {
    document.getElementById("orderPriceText").innerText = i18next.t("order.price");
}

// 언어 변경 함수
function changeLanguage(lang) {
    i18next.changeLanguage(lang, updateMessages);
}

// 버튼 이벤트 리스너
document.getElementById("changeToEnglish").addEventListener("click", function () {
    changeLanguage("en");
});

document.getElementById("changeToKorean").addEventListener("click", function () {
    changeLanguage("ko");
});
```

📌 **✅ HTML 버튼**


```html
<p id="orderPriceText"></p>
<button id="changeToEnglish">English</button>
<button id="changeToKorean">한국어</button>
```


```js
$(document).ready(function () {
    const locale = getCookie("localeCookie") || "ko"; // 쿠키가 없으면 기본값 'ko'
    console.log("쿠키에서 가져온 로케일:", locale);
    
    loadProperties(locale); // 쿠키에서 가져온 언어로 로드

    // 버튼 클릭 시 언어 변경 & 쿠키 저장
    $(".changeLang").on("click", function () {
        let lang = $(this).data("lang");
        document.cookie = "localeCookie=" + lang + "; path=/"; // 쿠키 저장
        loadProperties(lang);
    });
});

// 쿠키 값 가져오는 함수
function getCookie(name) {
    const cookies = document.cookie.split("; ");
    for (let cookie of cookies) {
        const [cookieName, cookieValue] = cookie.split("=");
        if (cookieName === name) {
            return cookieValue;
        }
    }
    return null; // 쿠키가 없으면 null 반환
}

// 다국어 메시지 불러오는 함수
function loadProperties(lang) {
    $.i18n.properties({
        name: "messages", // 파일명 (messages_ko.properties, messages_en.properties)
        path: "messages/", // properties 파일 경로
        mode: "map",
        language: lang, // 쿠키에서 가져온 언어 사용
        callback: function () {
            $("#orderPriceText").text($.i18n.prop("order.price"));
        }
    });
}
```


여기서 파일명은

common-message_en.properties 이 부분에서 common-message이 부분을 적어야한다
```js
function loadProperties(lang) {
    $.i18n.properties({
        name: "common-message", // 파일명 앞부분
        path: "messages/", // properties 파일 경로
        mode: "map",
        language: lang, // ex: en → common-message_en.properties
        callback: function () {
            $("#orderPriceText").text($.i18n.prop("order.price"));
        }
    });
}

```


되게끔 한 코드

```js
$.i18n.prop()
```
이건 실패로 다르게 가져옴

왜인지 모르겠으나 실패로

```js
$.i18n.map[""]
```
안에 키값 넣어서 가져옴

```js
function getCookie(name) {  
    const cookies = document.cookie.split("; ");  
    for (let cookie of cookies) {  
        const [cookieName, cookieValue] = cookie.split("=");  
        if (cookieName === name) {  
            return cookieValue;  
        }  
    }  
    return null; // 쿠키가 없으면 null 반환  
}  
  
  
function loadProperties(lang) {  
    console.log(lang)  
    if (typeof $.i18n === "undefined") {  
        console.error("❌ `jquery-i18n-properties`가 로드되지 않았습니다.");  
        return;  
    }  
    console.log("✅ jQuery 버전:", $.fn.jquery);  
    console.log("✅ jQuery i18n Properties 로드 확인:", typeof $.i18n);  
    if (typeof settings === "undefined") {  
        console.warn("⚠ `settings` 객체가 정의되지 않았습니다.");  
    }  
    $.i18n.properties({  
        name: "common-message", // 파일명 앞부분 (common-message_ko.properties, common-message_en.properties)        path: "/common/message/", // properties 파일 경로  
        mode: "map",  
        language: lang, // 쿠키에서 가져온 언어 사용  
        additionalParameters: { lang: lang },  
        callback : function(){  
            console.log("✅ 다국어 파일 로드 완료:", lang);  
            console.log("📌 $.i18n.map 데이터:", $.i18n.map);  
            console.log("📌 $.i18n.map 데이터2:", $.i18n.map["board.insert"]);  
            console.log("📌 현재 로드된 모든 키:", Object.keys($.i18n.map["board.insert"]));  
        }  
  
    });  
}  
$(document).ready(function () {  
  
    // `localeCookie` 값 가져오기  
    const locale = getCookie("localeCookie");  
    console.log("쿠키에서 가져온 로케일:", locale); // "en"  
    loadProperties(locale);

)}
```


컨트롤러

```java
@RequestMapping("/message/{filename}")  
@ResponseBody  
public String getMessageFile(@PathVariable String filename, @RequestParam(value = "lang", required = false) String lang) throws IOException {  
  
    log.info("lang {}",lang);  
    String filePath = "message/" + filename;  
    log.info("filePath {}",filePath);  
    Resource resource = new ClassPathResource(filePath);  
  
    if (!resource.exists()) {  
        throw new IOException("파일을 찾을 수 없습니다: " + filePath);  
    }  
  
    return new String(Files.readAllBytes(Paths.get(resource.getURI())));  
}
```