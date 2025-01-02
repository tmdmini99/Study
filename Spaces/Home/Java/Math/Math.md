
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


Math.pow()

	 --주어진 숫자를 제곱한다 
	 첫번째인자는 밑수이고, 두번째 인자는 지수
	 Math.pow(3,2) 이라고 하면 3의 2제곱이 계산

```java
int answer = (int) Math.pow(5, 4);
// 결과 625
```

Math.sqrt()
	 -- 주어진 숫자의 제곱근을  반환한다


```java
double a = 16;
double b = 121;
double c = 10;

System.out.println(Math.sqrt(a)); //출력 4.0
System.out.println(Math.sqrt(b)); //     11.0
System.out.println(Math.sqrt(c)); //     3.1622776601683795
```