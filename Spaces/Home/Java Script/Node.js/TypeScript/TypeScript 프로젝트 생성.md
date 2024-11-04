



`package.json` 파일을 생성하고 기본 정보를 추가합니다:
```bash
npm init -y
```


### 2. TypeScript 및 필요한 패키지 설치

TypeScript와 관련 도구를 설치합니다:

```bash
npm install typescript ts-node @types/node --save-dev
npm install express
npm install @types/express --save-dev
npm install --save-dev @types/debug
```


### 3. TypeScript 설정 파일 (`tsconfig.json`) 생성

프로젝트 루트에 TypeScript 설정 파일을 생성합니다:
```bash
npx tsc --init
```

생성된 `tsconfig.json` 파일을 열어 다음 설정을 업데이트합니다:

```json
{

  "compilerOptions": {

      "target": "es6",                // 컴파일할 자바스크립트 버전

      "module": "commonjs",           // 모듈 시스템

      "outDir": "./dist",             // 컴파일된 파일이 저장될 디렉토리

      "rootDir": "./src",             // 소스 파일이 위치한 디렉토리

      "strict": true,                  // 모든 엄격한 타입 검사를 활성화

      "esModuleInterop": true,         // ES6 모듈 호환성

      "skipLibCheck": true,            // 타입 검사 건너뛰기

      "forceConsistentCasingInFileNames": true, // 파일 이름 대소문자 일관성 유지

      "allowJs": true, // JavaScript 파일을 포함하도록 설정

      "checkJs": false // JavaScript 파일에 대해 타입 검사를 하지 않도록 설정

  },

  "include": [

      "src/**/*"                       // src 폴더 내 모든 파일 포함

  ],

  "exclude": [

      "node_modules",                  // node_modules 폴더 제외

      "dist"                           // dist 폴더 제외

  ]

}

```


### 5. Express 서버 파일 작성

#### `src/app.ts`


app.ts
```ts
// src/app.ts

import express from 'express';

  

const app = express();

  

// Middleware, Routes 설정 등을 여기서 추가

  

app.get('/', (req, res) => {

    res.send('Hello World!');

});

  

export default app;
```



`www.ts`

```ts
import app from './app';  // app.ts에서 app 가져오기
import debug from 'debug';
import http from 'http';

const port = normalizePort(process.env.PORT || '3000');
app.set('port', port);

const server = http.createServer(app);

server.listen(port);
server.on('error', onError);
server.on('listening', onListening);

function normalizePort(val: string): number | string | boolean {
    const port = parseInt(val, 10);
    if (isNaN(port)) return val; // named pipe
    if (port >= 0) return port;   // port number
    return false;
}

function onError(error: any) {
    if (error.syscall !== 'listen') throw error;

    const bind = typeof port === 'string' ? 'Pipe ' + port : 'Port ' + port;

    switch (error.code) {
        case 'EACCES':
            console.error(bind + ' requires elevated privileges');
            process.exit(1);
            break;
        case 'EADDRINUSE':
            console.error(bind + ' is already in use');
            process.exit(1);
            break;
        default:
            throw error;
    }
}

function onListening() {
    const addr = server.address();
    const bind = typeof addr === 'string' ? 'pipe ' + addr : 'port ' + addr.port;
    debug('Listening on ' + bind);
}

```


```bash
npx tsc

```


```bash
node dist/www.js
```


```bash
npm run build
```


```bash
npm start
```


### 2. **Nodemon과 TypeScript의 조합 사용하기**

`nodemon`을 사용하여 개발 중에 서버를 자동으로 재시작하도록 설정할 수 있습니다. `nodemon`은 파일의 변경을 감지하여 서버를 다시 시작합니다. TypeScript와 함께 사용하려면 `ts-node`와 함께 설정할 수 있습니다.

**의존성 설치**: `nodemon`과 `ts-node`를 개발 의존성으로 설치합니다.
```bash
npm install --save-dev nodemon ts-node
```

```bash
npm install --save-dev nodemon
```

```bash
npm install --save-dev ts-node
```


**Nodemon 설정**: `nodemon.json` 파일을 프로젝트 루트에 생성하여 다음과 같이 설정합니다:

```bash
typeTest/
├── node_modules/
├── dist/
├── src/
│   ├── index.ts
│   ├── app.ts
│   └── ...
├── package.json
├── tsconfig.json
└── nodemon.json  <-- 여기서 생성

```

```json
{
    "watch": ["src"],
    "ext": "ts",
    "exec": "ts-node ./src/routes/index.ts"
}
```

**Nodemon 실행**: `package.json`의 `scripts`에 `dev` 스크립트를 추가합니다.
```json
"scripts": {
    "build": "tsc",
    "watch": "tsc --watch",
    "dev": "nodemon"
}
```

그런 다음 다음 명령어로 서버를 실행합니다:
```bash
npm run dev
```
