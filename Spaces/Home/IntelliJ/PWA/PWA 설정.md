### IntelliJ IDEA 설정 확인

1. **IntelliJ IDEA 업데이트**:
    
    - IntelliJ IDEA의 최신 버전이 설치되어 있는지 확인하세요. 메뉴에서 **Help** > **Check for Updates**를 선택하여 최신 버전으로 업데이트할 수 있습니다.
2. **JavaScript 플러그인 활성화**:
    
    - IntelliJ IDEA에서 JavaScript 기능이 포함된 플러그인이 활성화되어 있는지 확인하세요.
    - **File** > **Settings** (Windows) 또는 **IntelliJ IDEA** > **Preferences** (macOS)로 이동합니다.
    - **Plugins**를 선택하고 `JavaScript and TypeScript` 플러그인이 활성화되어 있는지 확인하세요.

### 2. 새로운 프로젝트 생성

1. **새 프로젝트 만들기**:
    
    - IntelliJ IDEA를 열고 **File** > **New** > **Project**를 선택합니다.
2. **HTML 선택**:
    
    - 왼쪽 메뉴에서 **Static Web** 또는 **JavaScript** 옵션이 없다면 **HTML**을 선택하세요.
    - **Next** 버튼을 클릭합니다.
3. **프로젝트 세부정보 입력**:
    
    - 프로젝트 이름과 저장할 위치를 입력합니다.
    - **Finish** 버튼을 클릭하여 프로젝트를 생성합니다.

### 3. 기본 HTML 파일 생성

1. **HTML 파일 만들기**:
    
    - 프로젝트의 `src` 또는 루트 폴더를 우클릭하고 **New** > **HTML File**을 선택합니다.
    - 파일 이름을 `index.html`로 설정하고 기본 HTML 구조를 추가합니다.


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My PWA</title>
    <link rel="manifest" href="manifest.json">
</head>
<body>
    <h1>Welcome to My Progressive Web App!</h1>
    <p>This is a simple PWA example.</p>
    <script>
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('service-worker.js')
                .then(registration => {
                    console.log('Service Worker registered with scope:', registration.scope);
                })
                .catch(error => {
                    console.log('Service Worker registration failed:', error);
                });
            });
        }
    </script>
</body>
</html>

```


### 서비스 워커 및 매니페스트 파일 추가

1. **매니페스트 파일 생성**:
    
    - `src` 폴더를 우클릭하고 **New** > **JSON File**을 선택하여 `manifest.json` 파일을 생성합니다.
    - 아래 내용을 추가합니다.


```json
{
    "short_name": "MyApp",
    "name": "My Progressive Web App",
    "icons": [
        {
            "src": "images/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "images/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "start_url": "/index.html",
    "display": "standalone",
    "theme_color": "#ffffff",
    "background_color": "#ffffff"
}

```


**서비스 워커 파일 생성**:

- `src` 폴더를 우클릭하고 **New** > **JavaScript File**을 선택하여 `service-worker.js` 파일을 만듭니다. 아래 내용을 추가합니다.


```js
self.addEventListener('install', event => {
    console.log('Service Worker: Installed');
    event.waitUntil(
        caches.open('static-v1').then(cache => {
            return cache.addAll([
                '/',
                '/index.html',
                '/styles.css',
                '/images/icon-192x192.png',
                '/images/icon-512x512.png'
            ]);
        })
    );
});

self.addEventListener('activate', event => {
    console.log('Service Worker: Activated');
});

self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request)
        .then(response => {
            return response || fetch(event.request);
        })
    );
});
```


### . 아이콘 이미지 추가 및 CSS 파일 생성

1. **아이콘 이미지 추가**:
    
    - `src` 폴더 안에 `images` 폴더를 생성하고 필요한 아이콘 이미지 파일 (`icon-192x192.png`, `icon-512x512.png`)을 추가합니다.
2. **스타일 시트 추가 (선택 사항)**:
    
    - `src` 폴더를 우클릭하고 **New** > **CSS File**을 선택하여 `styles.css` 파일을 생성합니다.

### 6. PWA 실행 및 테스트

1. **내장 서버 실행**:
    
    - IntelliJ IDEA에서 내장 서버를 사용하여 PWA를 실행합니다. **Run** > **Edit Configurations**를 선택하여 새로운 JavaScript Debug 구성을 추가합니다.
2. **웹 브라우저에서 실행**:
    
    - 설정이 완료되면 **Run** 버튼을 클릭하여 프로젝트를 실행합니다. 브라우저에서 `http://localhost:63342`로 이동하여 PWA를 확인합니다.
3. **PWA 기능 테스트**:
    
    - Chrome DevTools의 Application 탭에서 매니페스트와 서비스 워커가 올바르게 등록되었는지 확인합니다. 오프라인 상태에서도 페이지가 작동하는지 테스트합니다.



## Node.js로 


### 1. IntelliJ IDEA 설치

- IntelliJ IDEA가 설치되어 있지 않다면 [JetBrains 공식 웹사이트](https://www.jetbrains.com/idea/download/)에서 다운로드하여 설치하세요.

### 2. Node.js 플러그인 설치 확인

1. IntelliJ IDEA를 열고, 상단 메뉴에서 `File` > `Settings`(Windows) 또는 `IntelliJ IDEA` > `Preferences`(macOS)를 클릭합니다.
2. 좌측 메뉴에서 `Plugins`를 선택한 후, `Marketplace` 탭에서 "Node.js"를 검색하여 설치합니다.
3. 설치 후, IDE를 재시작합니다.

### 3. 새로운 Node.js 프로젝트 만들기

1. IntelliJ IDEA를 열고, `File` > `New Project...`를 클릭합니다.
2. `Node.js`를 선택합니다. 이 옵션이 보이지 않으면, 위에서 설치한 Node.js 플러그인이 제대로 설치되지 않았을 수 있습니다.
3. `Project SDK`에서 사용할 Node.js 버전을 선택하거나 추가합니다.
4. 프로젝트 이름과 위치를 설정하고 `Finish`를 클릭합니다.

### 4. Express.js 설치 및 기본 설정

1. **패키지 초기화**:
    
    - 새로 생성된 프로젝트의 터미널을 열고 아래 명령어를 입력하여 `package.json` 파일을 생성합니다.


```bash
npm init -y
```

**Express.js 설치**:

- 같은 터미널에서 Express.js를 설치합니다.

```bash
npm install express
```

**기본 서버 설정**:

- 프로젝트 루트에 `server.js` 파일을 생성하고 아래 코드를 추가합니다.


```js
const express = require('express');
const path = require('path');

const app = express();
const PORT = process.env.PORT || 3000;

// 정적 파일 제공
app.use(express.static(path.join(__dirname, 'public')));

// 루트 경로에 대한 응답
app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});

```


### 5. PWA 설정

1. **정적 파일 및 매니페스트 파일 추가**:
    
    - 프로젝트 루트에 `public` 폴더를 생성합니다.
    - `public` 폴더 안에 `index.html`, `manifest.json`, `styles.css`, `service-worker.js` 파일을 생성합니다.
2. **파일 내용 추가**:
    
    - `index.html` 파일에 아래 내용을 추가합니다.




```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My PWA</title>
    <link rel="manifest" href="manifest.json">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Welcome to My Progressive Web App!</h1>
    <p>This is a simple PWA example.</p>
    <script>
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('service-worker.js')
                .then(registration => {
                    console.log('Service Worker registered with scope:', registration.scope);
                })
                .catch(error => {
                    console.log('Service Worker registration failed:', error);
                });
            });
        }
    </script>
</body>
</html>

```



`manifest.json` 파일에 아래 내용을 추가합니다.


```json
{
    "short_name": "MyApp",
    "name": "My Progressive Web App",
    "icons": [
        {
            "src": "images/icon-192x192.png",
            "sizes": "192x192",
            "type": "image/png"
        },
        {
            "src": "images/icon-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "start_url": "/index.html",
    "display": "standalone",
    "theme_color": "#ffffff",
    "background_color": "#ffffff"
}

```



`styles.css` 파일에 아래 내용을 추가합니다.



```css
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f0f0f0;
    color: #333;
}
```



`service-worker.js` 파일에 아래 내용을 추가합니다.

```js
self.addEventListener('install', event => {
    console.log('Service Worker: Installed');
    event.waitUntil(
        caches.open('static-v1').then(cache => {
            return cache.addAll([
                '/',
                '/index.html',
                '/styles.css',
                '/images/icon-192x192.png',
                '/images/icon-512x512.png'
            ]);
        })
    );
});

self.addEventListener('activate', event => {
    console.log('Service Worker: Activated');
});

self.addEventListener('fetch', event => {
    event.respondWith(
        caches.match(event.request)
        .then(response => {
            return response || fetch(event.request);
        })
    );
});
```


1. **아이콘 이미지 추가**:
    
    - `public/images` 폴더를 만들고 필요한 아이콘 이미지 파일(`icon-192x192.png`, `icon-512x512.png`)을 추가합니다.

### 6. 서버 실행

- IntelliJ IDEA의 터미널에서 아래 명령어를 입력하여 서버를 실행합니다.


```bash
node server.js
```


- 브라우저를 열고 `http://localhost:3000`으로 이동하여 PWA를 확인합니다.

### 7. PWA 기능 테스트

- Chrome DevTools의 Application 탭에서 매니페스트와 서비스 워커가 올바르게 등록되었는지 확인합니다.
- 오프라인 상태에서도 페이지가 작동하는지 테스트합니다.

### 추가 사항

- IntelliJ IDEA에서는 Git과 같은 버전 관리 시스템을 사용하여 프로젝트를 관리하는 것이 좋습니다.
- 추가적으로, 환경변수 설정이나 더 복잡한 라우팅 등도 구현할 수 있습니다.