
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

