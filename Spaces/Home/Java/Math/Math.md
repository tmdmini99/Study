
### floor vs trunc

공통점) 숫자의 소숫점 이하를 버린다.

차이점) 두 함수의 동작방식이 다르다.

- Math.trunc( )  
    -소숫점 이하를 단순히 잘라내버린다.

```javascript
    ex)  Math.trunc(3.14) = 3

         Math.trunc(-3.14) = -3
```

  

- Math.floor( )  
    -주어진 숫자보다 작거나 같은 가장 큰 정수를 반환한다.

```javascript
    ex) Math.floor(3.14) = 3

        Math.floor(-3.14) = -4
```

