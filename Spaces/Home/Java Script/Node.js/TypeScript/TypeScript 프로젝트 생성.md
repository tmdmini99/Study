



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
        "target": "es6",
        "module": "commonjs",
        "outDir": "./dist",
        "rootDir": "./src",
        "strict": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true
    },
    "include": [
        "src/**/*"  // src 폴더 내 모든 파일 포함
    ],
    "exclude": [
        "node_modules",
        "dist"
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

