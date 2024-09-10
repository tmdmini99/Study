**Node JS 변수 선언**

변수를 선언할때는 var 을 이용한다.

**예제1 - 일반 변수**

```js
// 변수 선언
var name = "hongku";
console.log(name);
console.log(typeof(name));​
```

name 이라는 변수명을 갖는 변수에 "hongku"라는 값을 할당한다.

그리고, console.log()를 이용해서 출력해본다.

typeof() 함수를 이용하면, 변수의 자료형 타입을 알 수 있다.

**결과**

```js
hongku
string
```

결과를 보면 알 수 있듯이,

name의 값은 hongku이고, 자료형은 string타입이다.

**예제2 - Object 변수**

```js
// 변수 선언
var student = {
    name:"hongku",
    age:26
};
// 출력
console.log(student);
console.log(typeof(student));
console.log(typeof(student.name));
console.log(typeof(student.age));​
```

이번에는 변수 안에 여러개의 값을 넣어 선언해보자.

student라는 변수명 안에 여러개의 속성(property)을 넣을 수 있다.

(여기서 속성은 name:"hongku" 또는 age:26 을 뜻한다.)

속성은 키(key)와 값(value)으로 나눠진다.

{ key : vlaue, key : value, ...... }

예)

{ name:"hongku", age:26 }

**결과**

```js
{ name: 'hongku', age: 26 }
object
string
number
```

student는 객체(object) 타입을 갖고, 그 안의 속성 값들은 값에 따라 string, number와 같은 타입을 가질 수 있다.

---
출처 - https://hongku.tistory.com/90