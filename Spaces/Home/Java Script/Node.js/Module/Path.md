


## Path 모듈 소개

Path 모듈은 파일과 Directory 경로 작업을 위한 Utility를 제공한다.
`path` 모듈은 Node.js에 내장되어 있어 있기 때문에 별도의 라이브러리 설치없이 바로 불러와서 사용할 수 있습니다.

CommonJS 모듈 시스템을 사용하는 프로젝트에서는 `require` 키워드로 불러오고, ES 모듈 시스템을 사용하는 프로젝트에서는 `import` 키워드를 사용할 수 있습니다.

```js
// CommonJS Modules
const path = require("path");
```

```js
// ES Modules
import path from "path";
```


`path` 모듈은 위에서 설명드린 운영체제 별로 상이할 수 있는 경로 관련 구분자를 `sep`와 `delimiter`라는 속성으로 제공

예를 들어, MacOS를 사용하고 있는 경우 다음과 같이 확인


```js
> { sep: path.sep, delimiter: path.delimiter }
{ sep: '/', delimiter: ':' }
```


Window
```js
> { sep: path.sep, delimiter: path.delimiter }
{ sep: '\\', delimiter: ';' }
```



### 1. path.normalize

normalize에 Path를 넣으면 알아서 경로를 normalize해서 return
`./`, `../`, `/` 문자를 남용한 경로는 파일 시스템에서 정확히 어느 위치를 나타내는지 햇갈리는 경우가 있다. 이럴 때는 `normalize()` 함수를 사용하여 불필요한 부분을 정리하여 경로를 단순화 할 수 있다
```js
const path = require("path");
let myPath = path.normalize("/this/is//a//my/.././path/normalize");

console.log(myPath); //   /this/is/a/path/normalize
```

위의 경우 ../는 상위 디렉토리로 가기 때문에 my가 생략

```js
> path.normalize("/Users/../daleseo//test.txt")
'/daleseo/test.txt'
```

### 2. path.join(\[...paths])

path.join은 String을 주게 되면 플랫폼별(windows or mac) 구분자를 사용해서 경로를 정규화해서 리턴

```js
const path = require("path");
myPath = path.join("/this", "is", "a", "////path//", "join");

console.log(myPath); //   /this/is/a/path/join
```


### 3. path.resolve(\[...paths])

path.resolve는 path.join과 path.normalize를 합친 것

이것은 주어진 문자열을 cd를 해서 최종 마지막 폴더까지 간 후 pwd(Print Working Directory)를 한 것과 동일 
그리고 문서에 따르면 절대 경로가 만들어질 때까지 prepend(추가)

그리고 만약 주어진 path를 모두 사용했음에도 절대 경로를 못만들었다면, cwd(Current working Directory)를 사용

```js
const path = require("path");
myPath = path.resolve("/this", "is/a", "../.", "path", "resolve");

console.log(myPath); //   /this/is/path/resolve

myPath = path.resolve("wwwroot", "static_files/png/", "../gif/image.gif");

console.log(myPath); //  /Users/yohan/Desktop/MyTest/wwwroot/static_files/gif/image.gif
/*
이 경우에는 주어진 값만으로는 절대경로를 만들 수 없으므로  cwd를 사용한다.
*/


```


```bash
$ path.resolve('/a', 'b', 'c')
'C:\a\b\c'
$ path.resolve('/a', '/b', 'c')
'C:\b\c'
$ path.resolve('/a', '/b', '/c')
'C:\c'
```

### 4. path.dirname(path), path.basename(path\[, ext])

path.dirname은 현재 작업하고 있는 디렉토리의 이름을 출력  
반면 path.basename은 파일이름을 출력 
만약 basename에 옵션값을 주게 되면 뒤의 확장자를 제거할 수 있음

```js
const path = require("path");
myPath = path.dirname("/foo/bar/baz/asdf/image.png");
console.log(myPath); ///foo/bar/baz/asdf

myPath = path.basename("/foo/bar/baz/asdf/image.png");
console.log(myPath); //image.png

myPath = path.basename("/foo/bar/baz/asdf/image.png", ".png");
console.log(myPath); //image
```

#### 파일 확장자 얻기


`extname()` 함수를 사용하면 주어진 경로에서 파일의 `.`을 포함한 확장자를 얻을 수 있음


```js
> path.extname("/Users/daleseo/test.txt")
'.txt'
```


### 절대 경로인지 알아내기


주어진 경로가 절대 경로인지 상대 경로인지 알아내려면 `isAbsolute()` 함수를 사용
```js
> path.isAbsolute("/Users/daleseo/test.txt")
true
> path.isAbsolute("./test.txt")
false
```

### 상대 경로 구하기

간혹 어떤 경로를 기준으로 다른 경로의 상대 경로를 알고 싶은 경우가 있습니다. 이럴 때는 `relative()` 함수에 기준의 되는 경로를 첫번째 인자로 대상이 되는 경로를 두번째 인자로 넘기면 상대 경로를 구해줍니다.

```js
> path.relative("/Users", "/Users/daleseo/test.txt")
'daleseo/test.txt'
```

```js
> path.relative("/Users/daleseo/test.txt", "/Applications")
'../../../Applications'
```




---
참조 - https://yohanpro.com/posts/js/node-path
https://www.daleseo.com/js-node-path/
