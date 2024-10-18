

- **서버에서 URL 인코딩:** Java에서 URL을 인코딩하려면 `URLEncoder` 클래스를 사용할 수 있습니다. 예를 들어, 서버에서 검색어를 URL 인코딩한 다음 클라이언트로 전송할 수 있습니다.
    
- **JavaScript에서 URL 생성:** JavaScript에서 URL을 생성할 때 서버에서 인코딩된 값을 사용할 수 있습니다.


```java
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;

// 검색어를 인코딩하는 메서드
public String encodeSearchTerm(String searchTerm) {
    try {
        return URLEncoder.encode(searchTerm, StandardCharsets.UTF_8.toString());
    } catch (Exception e) {
        // 오류 처리
        e.printStackTrace();
        return searchTerm; // 인코딩 실패 시 원래 값 반환
    }
}

```



```js
// 서버에서 인코딩된 검색어를 받아옵니다.
const encodedSearchTerm = /* 서버에서 제공하는 인코딩된 검색어 */;

fetch(url.toString() + `?search=${encodedSearchTerm}`, {
    method: 'GET'
})
.then(response => response.json())
.then(data => {
    const searchResults = document.getElementById('searchResults');
    searchResults.innerHTML = ''; // 기존 결과 지우기

    if (data.length > 0) {
        data.forEach(item => {
            searchResults.appendChild(createResultElement(item)); // 검색 결과 표시
        });
    } else {
        searchResults.innerHTML = '<div>검색 결과가 없습니다.</div>';
    }
});

// 결과를 생성하는 헬퍼 함수
function createResultElement(item) {
    const div = document.createElement('div');
    div.textContent = item.name; // item.name을 원하는 데이터로 변경
    return div;
}

```