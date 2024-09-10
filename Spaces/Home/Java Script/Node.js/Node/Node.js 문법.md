

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




# 😎 **require() : 해결 (resolve) 알고리즘**

```jsx
const myJsCode = require("./myJsCode"); // .js는 생략 가능하다.
const path = require("path");
const express = require("express");
```

해결 알고리즘은 크게 다음 세 가지로 나눌 수 있어요.

1. 파일 모듈 : 상대 경로로 작성되었는가?
2. 코어 모듈 : 파일 명만이 언급되었는가?
3. 패키지 모듈 : 코어 모듈에서 해당 파일을 찾을 수 없는가? ( => node_modules)

우리는 require로 이런 세 가지 형태를 불러올 수 있어요.

안타깝게도 이 모든 걸 하나로 정리해서 설명하기는 `많이 많이 많이` 힘들 거 같아요.

그래서 하나씩 순차적으로 설명해볼까 해요.

# 😘 **require() : 코어 모듈을 불러오는 방법**

실제 Node.js에서의 require는 이렇게 생겼어요.

```jsx
NativeModule.require = function (id) {
    if (id == "native_module") { // (1)
        return NativeModule;
    }
    var cached = NativeModule.getCached(id); // (2)
    if (cached) { // (3)
        return cached.exports;
    }
    if (!NativeModule.exists(id)) { // (4)
        throw new Error("No such native module " + id);
    }

    process.moduleLoadList.push("NativeModule " + id); // (5)
    var nativeModule = new NativeModule(id); // (6)
    nativeModule.cache();
    nativeModule.compile();
    return nativeModule.exports;
};
```

이건 아마도, native ( core )를 require() 하는 것으로 보여요.

각 라인에 대해서 설명할 필요가 있을 거 같아요.

그러니깐 주석의 넘버링을 봐주시면 될 거 같아요.

- (1) : 일단 native_module을 부르는 건지 봐야 해요. 그런거면 전체를 주면 돼요.
- (1) : 하지만 일반적으로는 필요 없을 거 같으니깐 이 코드는 설명할 때는 지워도 될 거 같아요.

```jsx
NativeModule.require = function (id) {
    var cached = NativeModule.getCached(id); // (2)
    if (cached) { // (3)
        return cached.exports;
    }
    if (!NativeModule.exists(id)) { // (4)
        throw new Error("No such native module " + id);
    }

    process.moduleLoadList.push("NativeModule " + id); // (5)
    var nativeModule = new NativeModule(id); // (6)
    nativeModule.cache();
    nativeModule.compile();
    return nativeModule.exports;
};
```

- (2) : 다음은 cache가 되었는지 찾아보는 거에요. 그냥 객체에 해당 key로 저장된 걸 찾는 거죠.
- (3) : 있으면 그냥 바로 꺼내주면 돼요.

간단해 보이지만, require()는 이런 식으로 cache를 쓰기 때문에,

여러 파일에서 require()를 사용하고, 동일한 걸 호출한다고 해도 1개의 모듈을 사용하게 강제해요.

```jsx
if (!NativeModule.exists(id)) { // (4)
	throw new Error("No such native module " + id);
}
```

- (4) : 이제 core 부분에 해당 id와 일치하는 게 있는지 체크하는 로직이에요.
- (4) : 이 부분은 직접 exist() 메서드를 확인해보는 게 좋을 거 같네요.

```jsx
const source = process.binding("natives");
const exists = (id) => {
    return NativeModule.source.hasOwnProperty(id);
};
```

- source는 process.binding에 'native'를 parameter로 전달한 결과물을 가지고 있어요.
- 여기에는 core에 해당하는 모든 코드가 텍스트 형태로 저장되어 있습니다.
- 따라서 exist는 id를 통해 그런 core 모듈이 있는지 찾는 거에요.
    - core는 readline이나 fs처럼 node.js 자체에 이미 있는 모듈을 말하는 거에요.

```jsx
NativeModule.require = function (id) {
    var cached = NativeModule.getCached(id); // (2)
    if (cached) { // (3)
        return cached.exports;
    }
    if (!NativeModule.exists(id)) { // (4)
        throw new Error("No such native module " + id);
    }

    process.moduleLoadList.push("NativeModule " + id); // (5)
    var nativeModule = new NativeModule(id); // (6)
    nativeModule.cache();
    nativeModule.compile();
    return nativeModule.exports;
};
```

- (5) : 다시 돌아와서, (5)는 현재 사용 중인 모듈을 기록해두는 것이고요,
- (6) : 이 부분은 그 부분에 대한 NativeModule을 생성해주는 거에요.

이후 캐시를 하고, 그 모듈을 컴파일하여 nativeModule.exports를 return 해주면 끝납니다.

(5), (6)보다는 cache와 compile 메서드들이 더 중요할 수 있는데요,

cache는 다음에도 동일한 모듈을 반환할 수 있도록 캐시해두는 역할을 하고요,

**compile은 단순 string 타입으로 저장되어 있는 모듈을, 실제 동작 가능한 형태로 변환하는 거에요.**

자세한 건 아래에서 이야기할게요.

# 🤗 module, module.exports?

놀랍게도 사실 얘네는 그저 구조분해할당에 불과합니다.

```jsx
// a.js

module.exports = { a : 3 };
```

```jsx
// b.js

// const { a } = require('./a.js');
const { a } = module.exports; // a.js에서의 module.exports
```

- 두번째 code box를 보면, 주석처리된 부분과 아래 코드는 사실 상 동일합니다.
- 결국 module.exports에서 구조분해할당해서 a를 받는 거에요.
- 왜 exports 할 때의 이름과 동일해야만 a를 받을 수 있느냐, 이것도 그럼 간단한 문제죠.
    - A : 구조분해할당이니깐요.

그러면 이번에는 이 코드들이 어떻게 만들어져 있는지를 보도록 할까요?

```jsx
const module = { exports : {a : 3} };

const loadModule = (filename, module, require) => {
    const wrappedSrc = `(function (module, exports, require) {
            ${fs.readFileSync(filename, "utf8")}
						// module.exports = { a : 3 }
            })(module, module.exports, kakasooRequire)`;
    eval(wrappedSrc);
};
```

- loadModule은 filename, module, require를 받고,
- 실행시킨 결과를 exports에 담도록 해주는 함수에요.
- exports에 담아놨으면, 결과적으로 return 해주지 않더라도 객체의 값은 유지가 되게 되므로,
- 실행된 함수의 결과물이 담기게 되는 거죠.

```jsx
const fs = require("fs");

function kakasooRequire(moduleName) {
    const loadModule = (filename, module, require) => {
        const wrappedSrc = `(function (module, exports, require) {
                ${fs.readFileSync(filename, "utf8")}
                })(module, module.exports, kakasooRequire)`;
        eval(wrappedSrc);
    };

    const id = moduleName;

    const module = {
        exports: {},
        id,
    };

    loadModule(id, module, kakasooRequire);
    return module.exports;
}
```

- 캐시도 없고, resolve()도 없다고 가정한 상태의 코드에요.
- 캐시가 없지만, 어차피 1번만 호출할 테니 저 상태로도 문제가 없을 거고요,
- resolve가 없지만, moduleName을 "./a.js"처럼 resolve된 형태로 넣어주면 문제가 없죠.
    - 원래는 const id = resolve(moduleName); 형태거든요.

그 다음에는 적당히 module 객체를 만든 다음에, loadModule을 호출하면 돼요.

그러면 loadModule에서 id에 맞는 모듈을 가져다가 랩핑된 함수를 실행시키게 돼요.

이 부분을 다시 한 번 보죠.

```jsx
const fs = require("fs");

const loadModule = (filename, module, require) => {
    const wrappedSrc = `(function (module, exports, require) {
            ${fs.readFileSync(filename, "utf8")}
            })(module, module.exports, kakasooRequire)`;
    eval(wrappedSrc);
};
```

- 랩핑이 되어 있는 부분만 자세히 보도록 해요.

```jsx
// a.js

module.exports = { a : 3 };
```

```jsx
(function(module, exports, require) {
	fs.readFileSync(filename, 'utf8')
})(module, module.exports, kakasoRequire);
```

- 만약 filename이 './a.js'라고 한다면, 위의 코드를 텍스트 상태로 읽게 되겠지만,
- eval 내에서는 이 텍스트가 JavaScript 코드로 평가되어서 실제 코드처럼 동작하게 되죠.
- 그러면 그 코드 에는 module.exports가 있으니까 담기게 돼요.
- 위의 두 코드 박스를 합치면,

```jsx
(function(module, exports, require) {
	module.exports = { a : 3 };
})(module, module.exports, kakasoRequire);
```

- 결국 이런 형태거든요.

그러면 이 코드 바깥에서도 module.exports의 값이 바뀌게 되고,

구조분해할당으로 값을 받는 게 가능해지죠, 결국 처음에 설명한 대로 되는 거에요.

```jsx
// b.js

const { a } = module.exports; // a.js에서의 module.exports
```

# 😗 전체 kakasooRequire 코드

적당히, require()을 흉내낼 수 있는 코드를 만들어봤어요.

```jsx
const fs = require("fs");

function kakasooRequire(moduleName) {
    const resolve = function (moduleName) {
        const moduleNameSplited = moduleName.split("");
        if (moduleNameSplited.slice(0, 2) === "./") {
            if (moduleNameSplited.slice(-3).join("") === ".js") {
							return moduleName;
            }
            return moduleName + ".js";
        } else {
					return moduleName;
				}
    };

    const loadModule = (filename, module, require) => {
        const wrappedSrc = `(function (module, exports, require) {
                ${fs.readFileSync(filename, "utf8")}
                })(module, module.exports, kakasooRequire)`;
        eval(wrappedSrc);
    };

    const id = resolve(moduleName);

    if (kakasooRequire.cache[id]) {
        return kakasooRequire.cache[id].exports;
    }

    const module = {
        exports: {},
        id,
    };

    kakasooRequire.cache[id] = module;
    loadModule(id, module, kakasooRequire);

    return module.exports;
}

kakasooRequire.cache = {};
```

- resolve를 추가한 모습이에요.
    - 실제로는 상대 경로, 절대 경로, 확장자 명, 코어 모듈, 없으면 node_modules까지 탐색하게끔 설계가 되어야 하지만, 여기서는 상대경로만 찾도록 되어 있어요.
    - 코어 모듈을 찾는 로직은 위에 있지만, 각각 별개로 만들어놔서 둘을 합쳐야 온전하겠죠?
- cache도 넣었어요, 그냥 빈 객체일 뿐입니다.

주목할 점은, loadModule에서의 id는 resolve된 결과물,

module은 id에 맞게 생성된 각각의 모듈, require은 require 자기 자신을 담은 파라미터라는 점이죠.

```jsx
module.exports = { a: 3 };
```

```jsx
const { a } = kakasooRequire("./test.js");

console.log(a); // 3
```

- 이 코드는 정상적으로 동작합니다!
- 코어 모듈을 먼저 만들어야 하는 이유가 사실, 그게 없으면 fs을 부를 수 없어서였어요.

# 🙄 더 알아보면 좋을 것들?

```jsx
eval(wrappedSrc);
```

- Q : 정말로 eval을 사용하고 있나요?

저도 아직 보는 중이긴 한데, NativeModule을 호출할 때에는 eval 대신에 runInThisContext 라고,

별도의 함수를 만들어서 사용하고 있어요.

```jsx
function runInThisContext(code, options) {
    const script = new ContextifyScript(code, options);
    return script.runInThisContext(); // 바깥의 runInThisContext()랑 다릅니다!
}
```

이런 형태를 하고 있는데요,

node.js의 vm처럼, 현재 코드가 실행되고 있는 context가 아닌 다른 context에서 코드를 실행시키고,

그 결과물을 module.exports에 담게 하는 거죠.

eval은 아무래도 문제 요소가 많아서 실제 코드에 사용하기는 좀 어렵지 않나 싶어요.



Node.js에서 전역적으로 제공되는 몇 가지 다른 유사한 변수와 객체가 있습니다. 이들은 현재 실행 중인 스크립트나 모듈의 경로, 파일명, 환경 정보 등을 제공합니다. 주요 전역 변수들은 다음과 같습니다:

### 1. **`__filename`**

- 현재 실행 중인 **스크립트 파일의 절대 경로**를 나타냅니다.
- `__dirname`과 달리, 파일의 전체 경로(파일명 포함)를 제공합니다.

**예시:**

```js
console.log(__filename);
// 출력 예시: /Users/username/project-directory/app.js

```

### 2. **`process.cwd()`**

- **현재 작업 디렉토리의 경로**를 반환합니다.
- `__dirname`과의 차이점은, `process.cwd()`는 **프로세스를 실행한 디렉토리**를 기준으로 하며, `__dirname`은 **스크립트 파일이 위치한 디렉토리**를 기준으로 합니다.
- 현재 디렉토리를 변경할 수 있는 `process.chdir()` 함수도 존재합니다.

**예시:**

```js
console.log(process.cwd());
// 출력 예시: /Users/username/another-directory (실행한 디렉토리)

```

### 3. **`require.main.filename`**

- 현재 애플리케이션에서 **첫 번째로 실행된 모듈(메인 모듈)의 파일 경로**를 나타냅니다.
- 보통 앱의 진입점인 첫 번째 실행 파일의 경로를 알고 싶을 때 사용합니다.

**예시:**

```js
console.log(require.main.filename);
// 출력 예시: /Users/username/project-directory/index.js

```

### 4. **`process.env`**

- **환경 변수**에 접근할 수 있는 객체입니다.
- 현재 시스템의 환경 변수나 사용자 정의 환경 변수를 가져오거나 설정할 때 사용합니다.

**예시:**

```js
console.log(process.env.NODE_ENV);

```

### 5. **`module`**

- 현재 모듈과 관련된 정보를 제공합니다.
- `module.filename`은 현재 모듈의 파일 경로를 나타냅니다.

**예시:**

```js
console.log(module.filename);
// 현재 파일의 전체 경로 출력
```

### `__dirname`의 역할:

- 현재 스크립트 파일이 속한 디렉토리의 경로를 반환합니다.
- 파일 경로를 동적으로 생성할 때 유용합니다. 예를 들어, 프로젝트의 특정 디렉토리에서 파일을 읽거나 쓸 때 사용됩니다.

### 예시


```js
const path = require('path');  
console.log(__dirname); 
// 출력 예시: /Users/username/project-directory`
```
### 요약:

- **`__dirname`**: 현재 파일이 위치한 디렉토리의 절대 경로.
- **`__filename`**: 현재 파일의 절대 경로(파일명 포함).
- **`process.cwd()`**: 프로세스가 실행된 디렉토리의 경로.
- **`require.main.filename`**: 첫 번째로 실행된 모듈의 경로.
- **`process.env`**: 환경 변수 정보.


> **변수 let 과 const**

  

ES6 이전에는 변수를 var을 사용했는데 이제 let (변수) 과 const (상수) 로 나누어진다.

var 과의 차이점은 var은 전역 범위인 반면 let 과 const 는 선언한 { } 내부 블록에서만 유효하다. 

  
```js
 function test () {

    let letNumber = 1;

    const constNumber = 1;

  

    if(true){

        let letNumber = 2;      

        const constNumber = 2; 

  

        console.log(letNumber);      // 2 출력

  console.log(constNumber);   // 2 출력

    }

  

	console.log(letNumber);      // 1 출력

  console.log(constNumber);   // 1 출력

}

```

  

  

  

> **템플릿 리터럴**

  

ES6 에서는 백틱 ( ` ) 을 이용하여 문자열을 + 기호 없이 간단히 처리할 수 있다.

또한 백틱 ( ` ) 안에서는 멀티 라인도 자유롭게 사용할 수 있다.

  
```
const id = "myId" ;

const url = `http://noahlog.tistory.com/login/${id}` ;

  

const message = ` 줄바꿈도 마음대로

  사용이 가능합니다. ` ;
```

  

  

  

> **함수의 기본 매개 변수값 지정**

  

ES6 의 함수는 매개변수에 기본값 (default) 을 지정해 줄 수 있다.

  

```
function test ( height =10 , width = 20, color = 'green' ) {

...

}
```

  

  

  

> **객체 리터럴**

  

ES6 에서는 객체 리터럴의 사용성이 좀 더 간편화 되었다.

  

```
var say = function() { console.log("hello"); }

  

const myObject = {

say,

work() { console.log("do work"); } ,

['name'] : 'noah'

};
```

  

  

  

> **화살표 함수**

  

ES6 에서는 축약형 함수인 화살표 함수를 사용할 수 있다. 

특이한 점이 있다면 화살표 함수에서 this 는 함수 자신을 호출하는 객체가 아닌

화살표 함수의 상위 스코프 (scope) 를 호출한다.

  

```
const numbers = [ 1, 2, 3, 4, 5 ];

  

let choice = numbers.map( v => v + 1);    //축약 형태로 return 도 생략 가능

  

const noah = {

name : "noah",

friend : [ "mimi" ],

printFriends() { 

this.friend.forEach( f => 

console.log(` ${this.name} loves ${f}`)); 

}

};

  

noah.printFriends();    // 결과값 : noah loves mimi
```

  

  

  

  

> **비구조화 할당 (destructuring Assignment)**

  

비구조화 할당은 배열이나 객체에서 데이터를 편리하게 가져올 수 있게 해준다.

  

```
//객체 

const blog = {

owner : "noah",

url : "noahlogs.tistory.com",

getPost() { console.log("ES6 문법 정리"); }

};

  

let { owner, getPost } = blog;       //각각 blog 객체의 owner , getPost() 의 데이터가 할당



//배열

const pocket = ["coin" , 10 , true];

const [ first , second, third ] = pocket;    //배열의 순서대로 값이 할당
```



#  filter 란?

자바스크립트에서 `filter` 는 배열에 사용하며, 주어진 함수를 만족하는 모든 요소를 모아 새 배열로 반환한다.

아래 코드를 통해 `filter` 가 어떻게 사용되는지 간단히 알아보자.

```null
let array = [3,5,11,0,9,'string'];
let result = array.filter((value) => value < 10)
console.log(result);

//output : array [3, 5, 0, 9]
```

위의 코드를 보면, array라는 배열이 있다. 이 배열에서 10미만의 값만 추출하려고 한다.  
이를 위해 우선 결과값들을 담아둘 `result` 변수를 만들었다. 그 후 기존 배열에 `.filter()`를 붙이고 괄호 안에는 **필터조건이 되는 함수**를 넣어 주었다.

#  filter 사용법

`filter` 를 쓸 때, 조건식을 filter()의 괄호 안에 쓰는 방법이 있고 filter외부에 사용하는 방법이 있다.

![[Pasted image 20240910145736.jpg]]

## 함수를 filter 내에 주기

위 사진의 **#1 solution**을 보면, `filter` 메서드의 조건식을 괄호 안에 바로 작성해 주었다.  
이 방법은 **조건식이 간단할 경우** 유용하며, 코드 라인을 줄일 수 있고 한 눈에 보기 편한 장점이 있다.

## 함수를 filter 외부에 사용

위 사진의 **#2 solution** 을 보면, `filter` 의 조건을 외부함수로 선언한 후 callback하고 있다.  
이 방법은 filter 조건식이 복잡해질 수록 코드 관리에 유용할 것 같다.



---

출처 -  https://noahlogs.tistory.com/26
