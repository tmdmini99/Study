**Node.js 함수 선언**

**방법 1**

```js
// 함수 선언 - 방법 1
function sum(num1, num2){
    return num1 + num2;
}
// 출력
console.log(sum(1,5));​
```

함수를 만들어 함수명을 이용해서, 호출하여 사용한다.

**결과**

```js
6
```

**방법 2**

```js
// 함수선언 - 방법 2
var print = function(){
    console.log("Hello Node");
}
// 호출
print()​
```

변수에 넣어서 그 변수를 통해 함수를 호출하는 방법이 있다.

**결과**

```js
Hello Node
```

**값을 반환하지 않는 함수의 경우**

```js
// 리턴하는 값이 없을경우
function no_return_func(){
 
}
console.log(no_return_func());​
```

return을 하지 않는 경우에는 함수를 호출하면 undefined 라는 값이 나온다.

**결과**

```js
undefined
```

**참고**

```js
var print = function(){
    console.log("Hello Node");
}
// 1초 후에 print 실행
setTimeout(print, 1000);​
```

```js
Hello Node
```

setTimeout() 함수를 이용하면, 자신이 설정한 시간 뒤에 함수를 호출 시킬 수 있다.




## 화살표 함수


`const readAllCsvFilesInFolder = (folderPath, columns) => { ... }`는 **화살표 함수**를 사용한 함수 선언을 나타냅니다. 이 구문은 ES6 (ECMAScript 2015)에서 도입된 **화살표 함수**의 문법을 사용하고 있습니다. 구체적으로 다음과 같은 의미를 가집니다:

### 화살표 함수 (Arrow Function)

1. **문법**:

```js
const 함수이름 = (매개변수1, 매개변수2) => {
  // 함수 본문
};
```
- 여기서 =>는 함수 본문을 시작하는 화살표입니다. 이는 함수 표현식을 간결하게 작성할 수 있게 해줍니다.
    
- **예시**:
```node
const add = (a, b) => {
  return a + b;
};
// 또는
const add = (a, b) => a + b;

```

위의 두 예시는 동일한 기능을 수행하는 화살표 함수를 나타냅니다. 두 번째 예시는 `return` 문을 생략한 간단한 형태입니다.

### 화살표 함수(Arrow Function).

- 화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 간략한 방법으로 함수를 선언 할 수 있습니다.
- 하지만 모든 경우 화살표 함수를 사용할 수 있는 것은 아닙니다. 그 이유는 아래의 차이점을 확인 하시면 됩니다.

#### [](https://shinsangeun.github.io/posts/nodejs/arrow-function#1-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98-%EC%84%A0%EC%96%B8)1. 화살표 함수 선언

```javascript
// 1. 매개변수 지정 방법
() => { ... }       // 매개변수가 없을 경우
x => { ... }        // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... }   // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없다.

// 함수 몸체 지정 방법
x => { return x * x }   // single line block
x => x * x              // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.

() => { return { a: 1 }; }
() => ({ a: 1 })        // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다.

() => {                 // multi line block.
  const x = 10;
  return x * x;
};
```

#### [](https://shinsangeun.github.io/posts/nodejs/arrow-function#2-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98-%ED%98%B8%EC%B6%9C)2. 화살표 함수 호출

- 화살표 함수는 익명 함수로만 사용할 수 있습니다. 따라서 화살표 함수를 호출하기 위해서는 함수 표현식을 사용합니다.
- ES5 버전

```javascript
var pow = function (x) { return x * x; };
console.log(pow(10)); // 100
```

- ES6 버전

```javascript
const pow = x => x * x;
console.log(pow(10)); // 100
```

- 또는, 콜백 함수로 사용할 수 있습니다. 이 경우 일반적인 함수 표현식 보다 간결합니다.
- ES5 버전

```javascript
var arr = [1, 2, 3];
var pow = arr.map(function (x) {
  return x * x;
});

console.log(pow); 
// [ 1, 4, 9 ]
```

- ES6 버전
 ```javascript
const arr = [1, 2, 3];
const pow = arr.map(x => x * x);

console.log(pow); // [ 1, 4, 9 ]
```

### 차이점.

- 일반 함수와 화살표 함수의 차이점은 아래와 같습니다.

#### 1. this

- 일반 함수와 화살표 함수의 가장 큰 차이점은 this 입니다.
- 화살표 함수와 일반 함수는 this가 다른 곳을 가리킵니다.
- 화살표 함수의 this는 바로 **상위 스코프의 this**와 같습니다.
- 일반 함수는 this가 **동적으로  바인딩** 됩니다. 일반 함수의 this는 `내부 함수, 콜백 함수: 전역 객체`, `객체의 메소드`, `생성자 함수` 입니다.

#### 2. 생성자 함수로 사용 가능 여부

- 일반 함수는 생성자 함수로 사용할 수 있지만, 화살표 함수는 생성자 함수로 사용할 수 없습니다.
- 화살표 함수는 prototype 프로퍼티를 가지고 있지 않기 때문 입니다.

#### 3. arguments 사용 가능 여부

- 일반 함수 에서는 함수가 실행 될때 암묵적으로 arguments 변수가 전달되어 사용할 수 있습니다.
- 화살표 함수에서는 arguments 변수가 전달되지 않습니다.




---
출처 - https://hongku.tistory.com/91
https://shinsangeun.github.io/posts/nodejs/arrow-function