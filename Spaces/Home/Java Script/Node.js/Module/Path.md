


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

















---
참조 - https://yohanpro.com/posts/js/node-path
https://www.daleseo.com/js-node-path/
