
## SummerNote

**ummernote**는 웹에서 WYSIWYG(What You See Is What You Get) 편집기를 구현하기 위한 **오픈소스 JavaScript 라이브러리**입니다. 즉, 사용자가 웹 페이지에서 텍스트를 입력하고, 서식을 지정하고, 이미지를 삽입하는 등의 작업을 시각적으로 할 수 있게 해주는 편집기입니다.

### Summernote의 주요 특징

1. **WYSIWYG 편집기**:
    
    - 사용자는 텍스트를 입력하고, 글꼴, 크기, 색상, 굵기, 이탤릭체 등을 시각적으로 설정할 수 있습니다. 그 외에도 이미지를 삽입하거나 링크를 추가하는 등 다양한 기능을 제공합니다.
2. **사용자 친화적인 인터페이스**:
    
    - Summernote는 기본적으로 리치 텍스트 편집기 기능을 제공하며, 버튼 클릭만으로 서식 설정이 가능합니다. 텍스트 편집기에서 사용자가 원하는 스타일을 시각적으로 적용할 수 있기 때문에 직관적입니다.
3. **확장 가능성**:
    
    - Summernote는 플러그인 형태로 기능을 추가할 수 있으며, 다양한 플러그인들이 제공되고 있습니다. 예를 들어, 파일 업로드, 이미지 삽입, 링크 삽입 등 다양한 기능을 쉽게 추가할 수 있습니다.
4. **모바일 친화성**:
    
    - Summernote는 모바일 장치에서 잘 작동하는 반응형 디자인을 지원합니다. 그래서 모바일에서 편집기를 사용하더라도 문제가 없습니다.
5. **HTML/CSS 출력**:
    
    - Summernote는 입력된 내용을 HTML 코드로 변환하여 저장하거나 서버로 전송할 수 있습니다. 사용자가 만든 콘텐츠는 HTML 형식으로 서버로 전송되며, 이를 통해 서버에서 쉽게 처리할 수 있습니다.
6. **사용자 정의 가능**:
    
    - 다양한 옵션을 통해 Summernote의 기능을 쉽게 커스터마이즈 할 수 있습니다. 예를 들어, 툴바에 어떤 버튼을 추가할지, 어떤 형식으로 출력할지를 설정할 수 있습니다.
7. **파일 및 이미지 업로드**:
    
    - Summernote는 텍스트 외에도 이미지, 파일을 업로드하는 기능을 지원합니다. 드래그 앤 드롭 또는 버튼 클릭을 통해 쉽게 파일을 삽입할 수 있습니다.

### Summernote의 주요 사용 사례

- **블로그 작성기**: 블로그나 게시판 시스템에서 글을 작성하는 편집기로 사용됩니다.
- **관리자 대시보드**: 관리자가 콘텐츠를 생성할 수 있는 편집기로 사용됩니다.
- **이메일 작성기**: 이메일 클라이언트에서 HTML 이메일을 작성하는 데 사용됩니다.
- **콘텐츠 관리 시스템(CMS)**: 웹사이트의 콘텐츠를 관리하는 데 사용됩니다.


```html
<!-- Summernote를 위한 스타일 시트 -->
<link href="https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote-bs5.min.css" rel="stylesheet">

<!-- Summernote 편집기 -->
<textarea id="summernote"></textarea>

<!-- Summernote 관련 스크립트 -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote-bs5.min.js"></script>

<script>
    $(document).ready(function() {
        $('#summernote').summernote({
            height: 300, // 편집기 높이
            toolbar: [
                ['style', ['bold', 'italic', 'underline', 'clear']],
                ['para', ['ul', 'ol', 'paragraph']],
                ['insert', ['link', 'picture']]
            ],
        });
    });
</script>
```


위의 예시에서, `#summernote`라는 ID를 가진 텍스트 영역을 Summernote 편집기로 변환하고, 툴바를 설정하는 방식으로 사용됩니다. `bold`, `italic`, `underline` 등의 버튼을 통해 텍스트 서식을 지정할 수 있고, 이미지를 삽입하는 등의 기능도 활성화됩니다.

### Summernote 사용 시 고려해야 할 점

1. **의존성**: Summernote는 jQuery와 Bootstrap을 필요로 합니다. 이 두 라이브러리가 없으면 Summernote가 제대로 작동하지 않을 수 있습니다. 따라서 HTML 페이지에서 적절한 버전의 jQuery와 Bootstrap을 먼저 로드해야 합니다.
    
2. **파일 업로드 기능**: Summernote는 파일 업로드 기능을 기본적으로 제공하지 않지만, 관련 플러그인을 통해 파일 업로드 기능을 추가할 수 있습니다.
    
3. **브라우저 호환성**: Summernote는 최신 브라우저에서 잘 작동하지만, 구형 브라우저에서는 일부 기능이 동작하지 않을 수 있습니다.
    
4. **반응형 디자인**: 모바일에서도 편집기를 사용할 수 있도록 반응형 웹 디자인이 지원되지만, 화면 크기나 요구 사항에 따라 추가적인 설정이 필요할 수 있습니다.


bootstrap5와 사용시

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css">
<link href="https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote-bs5.min.css" rel="stylesheet">



<script src="https://stackpath.bootstrapcdn.com/bootstrap/5.3.0-alpha1/js/bootstrap.bundle.min.js"></script>  
<script src="https://cdn.jsdelivr.net/npm/summernote@0.8.20/dist/summernote-bs5.min.js"></script>


```