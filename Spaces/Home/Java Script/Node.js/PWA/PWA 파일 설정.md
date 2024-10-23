


### Node.js로 PWA 만들기

여기서는 Node.js와 Express.js를 사용하여 기본적인 PWA를 만드는 과정을 설명하겠습니다.

#### 1. Node.js 설치

Node.js가 설치되어 있지 않다면 [Node.js 공식 웹사이트](https://nodejs.org/)에서 설치해 주세요.

#### 2. 프로젝트 설정

1. **새 프로젝트 폴더 만들기**:

```bash
mkdir my-pwa
cd my-pwa
```


**npm 초기화**:

```bash
npm init -y
```


**Express.js 설치**:
```bash
npm install express
```

#### 3. 기본 서버 설정

1. **`server.js` 파일 만들기**: 프로젝트 루트에 `server.js` 파일을 생성하고 아래 코드를 추가합니다.


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


#### 4. 정적 파일 및 PWA 설정

1. **`public` 폴더 만들기**:

```bash
mkdir public
```


**HTML 파일 생성**: `public` 폴더 안에 `index.html` 파일을 생성하고 아래 코드를 추가합니다.


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


**매니페스트 파일 생성**: `public` 폴더 안에 `manifest.json` 파일을 만들고 아래 내용을 추가합니다.


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


**스타일 시트 추가**: `public` 폴더 안에 `styles.css` 파일을 만들고 아래 내용을 추가합니다.

```js
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f0f0f0;
    color: #333;
}
```


**서비스 워커 파일 생성**: `public` 폴더 안에 `service-worker.js` 파일을 만들고 아래 내용을 추가합니다.

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


1. **아이콘 이미지 추가**: `public/images` 폴더를 만들고 필요한 아이콘 이미지 파일(`icon-192x192.png`, `icon-512x512.png`)을 추가합니다.
    

#### 5. 서버 실행

- 아래 명령어를 통해 서버를 실행합니다.


```bash
node server.js
```


- 브라우저를 열고 `http://localhost:3000`으로 이동하여 PWA를 확인합니다.

### 6. PWA 기능 테스트

- Chrome DevTools의 Application 탭에서 매니페스트와 서비스 워커가 올바르게 등록되었는지 확인합니다.
- 오프라인 상태에서도 페이지가 작동하는지 테스트합니다.

### 7. 추가 옵션

- Node.js와 Express.js 외에도 다른 백엔드 프레임워크를 사용할 수도 있습니다. 예를 들어, Koa, Hapi 등 다양한 선택지가 있습니다.
- 데이터베이스를 추가하여 동적인 데이터를 처리할 수도 있습니다.