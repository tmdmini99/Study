

```js
var express = require('express');  
var router = express.Router();  
  
/* GET home page. */  
router.get('/', function(req, res, next) {  
  res.render('index', { title: 'Express' });  
});  
  
module.exports = router;
```


### 1. **router.get('/')**

- 이 부분은 Express에서 **라우터(router)**를 사용하여 HTTP GET 요청을 처리하는 경로를 정의한 것입니다.
- `'/'`는 루트 경로를 나타내며, 이 경로에 GET 요청이 들어올 때 해당 핸들러가 실행됩니다.
- 예를 들어, 사용자가 **\http://localhost:3000/**과 같은 URL에 접근할 때, 이 경로에 해당하는 핸들러가 호출됩니다.

### 2. **function(req, res, next)**

- 이 함수는 **미들웨어 함수**로, GET 요청이 들어왔을 때 실행되는 핸들러입니다. 세 가지 매개변수를 받습니다:
    - `req`: **요청 객체(request)**로, 클라이언트로부터 받은 데이터를 담고 있습니다.
    - `res`: **응답 객체(response)**로, 서버가 클라이언트에게 데이터를 보낼 때 사용됩니다.
    - `next`: 다음 미들웨어 함수를 호출하는 역할을 합니다. 이 경우에는 사용되지 않지만, 여러 미들웨어가 연결될 때 유용합니다.

### 3. **res.render('index', { title: 'Express' })**

- `res.render()`는 서버에서 **뷰를 렌더링(render)**하는 함수입니다. 즉, 템플릿 엔진(예: Pug, EJS 등)을 사용해 동적으로 HTML 페이지를 생성해 응답합니다.
- `'index'`는 렌더링할 **템플릿 파일**의 이름을 의미합니다. 여기서는 `views` 디렉터리에 있는 `index.pug`나 `index.ejs` 파일을 렌더링할 가능성이 높습니다.
- `{ title: 'Express' }`는 템플릿에 전달할 **데이터 객체**입니다. 여기서 템플릿 파일 안에서 `title` 변수를 사용하여 `'Express'`라는 값을 출력할 수 있습니다.




`module.exports = router;`는 **Node.js의 모듈 시스템**에서 사용되는 코드로, 현재 모듈(파일)에서 정의된 `router` 객체를 외부에서 사용할 수 있도록 **내보내는(export)** 역할을 합니다.

이 코드가 하는 일은 다음과 같습니다:

### 1. **`module.exports`란?**

- **`module.exports`**는 Node.js의 각 파일이 하나의 모듈로 간주되며, 그 모듈에서 다른 파일로 내보낼 수 있는 객체나 값을 설정하는 데 사용됩니다.
- 즉, `module.exports`에 할당된 값은 이 모듈을 **`require()`**로 가져올 때 반환됩니다.

### 2. **`router` 객체**

- 코드에서 `router`는 Express.js에서 **라우터 객체**를 의미하며, 라우팅 경로 및 핸들러를 설정하는 데 사용됩니다.
- 예를 들어, 라우터는 GET, POST 등의 요청에 대해 경로와 응답을 정의하는 역할을 합니다.

### 3. **`module.exports = router;`의 역할**

- 이 코드는 **현재 파일에서 정의된 `router`를 다른 파일에서 사용할 수 있도록 내보내는 것**입니다.

