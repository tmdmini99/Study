### 일 구조 및 역할

1. **`app.js`**: Express 애플리케이션을 설정하는 파일입니다. 미들웨어와 라우터 등을 등록하고, 서버와 관련된 직접적인 코드는 없습니다.
    
2. **`bin/www.js`**: Node.js 서버를 시작하는 파일입니다.
    
    - 여기서 HTTP 서버를 만들고, `app`을 인수로 넘겨서 Express 앱을 실행합니다.
    - 포트와 네트워크 인터페이스를 설정하고, 에러 및 리스닝 이벤트에 대한 처리를 포함하고 있습니다.

### `www.js` 파일의 주요 기능

```js
var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);
```


```js
var server = http.createServer(app);
```


```js
server.listen(port);
```


### 왜 `app.listen()`이 없어도 되는가?

이 구조에서는 `app.listen()` 대신에 HTTP 서버를 생성하여 리스닝하는 방식으로 서버를 실행하기 때문에, `app.listen()`을 사용할 필요가 없습니다. `www.js` 파일에서 HTTP 서버가 Express 앱을 사용하고 있기 때문에, 동일한 결과를 얻는 것입니다.

### 실행 방법

- 이 구조에서는 터미널에서 `npm start` 명령어를 사용하여 `www.js` 파일이 실행되고, 그 결과로 Express 애플리케이션이 서버로 실행됩니다. 이 명령어는 일반적으로 `package.json`에 설정되어 있습니다.


### CORS 문제 확인

만약 클라이언트와 서버가 다른 출처(도메인)에서 실행되고 있다면, CORS(Cross-Origin Resource Sharing) 문제로 인해 요청이 차단될 수 있습니다. 이 경우 Express 서버에 CORS 미들웨어를 추가하여 요청을 허용해야 합니다.

app.js
```js
const cors = require('cors');
app.use(cors()); // CORS 허용
```

