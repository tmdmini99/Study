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