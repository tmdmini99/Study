**Node.js 함수 선언**

**방법 1**

```
// 함수 선언 - 방법 1
function sum(num1, num2){
    return num1 + num2;
}
// 출력
console.log(sum(1,5));​
```

함수를 만들어 함수명을 이용해서, 호출하여 사용한다.

**결과**

```
6
```

**방법 2**

```
// 함수선언 - 방법 2
var print = function(){
    console.log("Hello Node");
}
// 호출
print()​
```

변수에 넣어서 그 변수를 통해 함수를 호출하는 방법이 있다.

**결과**

```
Hello Node
```

**값을 반환하지 않는 함수의 경우**

```
// 리턴하는 값이 없을경우
function no_return_func(){
 
}
console.log(no_return_func());​
```

return을 하지 않는 경우에는 함수를 호출하면 undefined 라는 값이 나온다.

**결과**

```
undefined
```

**참고**

```
var print = function(){
    console.log("Hello Node");
}
// 1초 후에 print 실행
setTimeout(print, 1000);​
```

```
Hello Node
```

setTimeout() 함수를 이용하면, 자신이 설정한 시간 뒤에 함수를 호출 시킬 수 있다.




## 화살표 함수






---
출처 - https://hongku.tistory.com/91
