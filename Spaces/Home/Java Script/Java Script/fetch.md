
### 1. **HTML 페이지**

- **예시**: `text/html`
    
- **설명**: HTML 문서 형식으로 데이터를 전송할 때 사용됩니다. 예를 들어, 웹 페이지를 요청하거나 제출할 때 사용합니다.
    
```js
fetch('/your/page', {
    method: 'GET',
    headers: { 'Content-Type': 'text/html' }
});
```

### 2. **JSON 데이터**

- **예시**: `application/json`
    
- **설명**: JSON 형식으로 데이터를 전송할 때 사용됩니다. API 요청에서 주로 사용되며, 클라이언트와 서버 간의 데
- 이터 교환에 많이 사용됩니다.


```js
fetch('/api/data', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ key: 'value' })
});
```

### 3. **폼 데이터**

- **예시**: `application/x-www-form-urlencoded`
    
- **설명**: HTML 폼을 통해 데이터를 전송할 때 기본적으로 사용되는 형식입니다. 키-값 쌍으로 인코딩된 데이터입니다.


```js
const formData = new URLSearchParams();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

fetch('/submit/form', {
    method: 'POST',
    headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
    body: formData.toString()
});

```

### 4. **파일 업로드**

- **예시**: `multipart/form-data`
    
- **설명**: 파일을 포함한 폼 데이터를 전송할 때 사용됩니다. 이 형식은 여러 종류의 데이터를 포함할 수 있도록 설계되었습니다.
    

```js
const formData = new FormData();
formData.append('file', fileInput.files[0]);

fetch('/upload', {
    method: 'POST',
    body: formData // Content-Type 자동 설정
});

```
### 5. **XML 데이터**

- **예시**: `application/xml` 또는 `text/xml`
    
- **설명**: XML 형식으로 데이터를 전송할 때 사용됩니다. XML 기반의 API와 데이터 전송에서 주로 사용됩니다.
    


```js
const xmlData = `<note><to>Tove</to><from>Jani</from><heading>Reminder</heading><body>Don't forget me this weekend!</body></note>`;

fetch('/api/xml', {
    method: 'POST',
    headers: { 'Content-Type': 'application/xml' },
    body: xmlData
});

```

### 6. **브라우저 캐시 요청**

- **예시**: `application/octet-stream`
    
- **설명**: 일반적인 이진 데이터 전송에 사용되며, 어떤 형식인지 모를 때 사용됩니다.


```js
fetch('/download', {
    method: 'GET',
    headers: { 'Content-Type': 'application/octet-stream' }
});

```
   

### 7. **커스텀 형식**

- **예시**: `application/vnd.example+json`
    
- **설명**: 특정 API나 데이터 형식에 대해 커스텀 미디어 타입을 정의할 수 있습니다. 예를 들어, 특정 버전의 API에서 사용할 수 있습니다.


```js
fetch('/api/v2/resource', {
    method: 'GET',
    headers: { 'Content-Type': 'application/vnd.example+json' }
});
```





```js
// Fetch를 사용하여 POST 요청 보내기
            fetch("https://jsonplaceholder.typicode.com/posts", {
                method: "POST",  // POST 방식
                headers: {
                    "Content-Type": "application/json; charset=UTF-8"  // JSON 형식 지정
                },
                body: JSON.stringify(postData)  // 데이터를 JSON 문자열로 변환하여 전송
            })
            .then(response => response.json())  // JSON 응답을 JavaScript 객체로 변환
            .then(data => {
                // 성공적으로 응답 데이터를 받았을 때
                document.getElementById("responseResult").innerHTML = `
                    <h3>Post Created Successfully</h3>
                    <p><strong>ID:</strong> ${data.id}</p>
                    <p><strong>Title:</strong> ${data.title}</p>
                    <p><strong>Body:</strong> ${data.body}</p>
                `;
            })
            .catch(error => {
                // 에러 발생 시
                document.getElementById("responseResult").innerHTML = "<p>Something went wrong.</p>";
                console.error("Error:", error);
            });
        });
```



fetch 요청 취소

```js
let abortController; // 전역 변수로 설정  
let isLoading = false; // 로딩 상태 플래그  
  
$(document).on('input', '.Polaris-TextField__Input', async function() {  
    // 이전 요청이 진행 중이면 취소  
    if (abortController) {  
        // 요청 취소를 백엔드에 알림  
        const requestId = abortController.requestId; // 현재 요청의 ID        await fetch(`/cancel`, {  
            method: 'GET'  
        });  
  
        abortController.abort(); // 이전 요청 취소  
        console.log('Previous request aborted');  
    }  
  
    // 새로운 AbortController 생성  
    abortController = new AbortController();  
    const signal = abortController.signal;  
  
    const searchTerm = $(this).val(); // 입력값 가져오기  
    console.log('입력된 값:', searchTerm);  
  
    const url = new URL(window.location.href);  
    if (searchTerm) {  
        url.searchParams.set('search', searchTerm);  
    } else {  
        url.searchParams.delete('search');  
    }  
  
    // pageNumber 파라미터 삭제  
    url.searchParams.delete('pageNumber');  
  
    // 로딩 바 표시  
    showLoading(); // 로딩 바 표시  
    isLoading = true; // 로딩 상태 설정  
  
    try {  
        const response = await fetch(url.toString(), {  
            method: 'GET',  
            signal: signal // signal 추가  
        });  
  
        if (response.ok) {  
            const data = await response.text();  
            const searchResults = $('.Polaris-IndexTable');  
            searchResults.empty(); // 기존 결과 지우기  
  
            // HTML 내용으로 업데이트  
            let newDocument = $.parseHTML(data);  
            let newContent = $(newDocument).find('.Polaris-IndexTable');  
  
            if (newContent.length > 0) {  
                searchResults.html(newContent.html()); // 결과 업데이트  
            } else {  
                searchResults.html('<div>검색 결과가 없습니다.</div>');  
            }  
  
            // URL 업데이트  
            history.pushState(null, '', url.toString());  
        }  
    } catch (error) {  
        if (error.name === 'AbortError') {  
            console.log('요청이 취소되었습니다.'); // 요청이 취소된 경우  
        } else {  
            console.error('검색 요청 실패:', error);  
            const searchResults = $('.Polaris-IndexTable');  
            searchResults.html('<div>검색 중 오류가 발생했습니다.</div>');  
        }  
    } finally {  
        // 요청 완료 후 로딩 바 숨김  
        if (isLoading) {  
            hideLoading(); // 로딩 바 숨김  
            isLoading = false; // 로딩 상태 리셋  
        }  
    }  
});
```