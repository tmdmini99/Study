
## Integer


### 사용자 정의 클래스

```java
class Box<T> {
    T value;
    void set(T value) { this.value = value; }
    T get() { return value; }
}

Box<String> stringBox = new Box<>();
Box<Integer> intBox = new Box<>();
```


### Integer

#### 진법 변환

```java
int num = 10;
String ternary = Integer.toString(num, 3);
System.out.println(ternary);  // 출력: 101\
```

`Integer.toString(숫자, 진법)`  
→ 원하는 진법(2~36 사이)으로 숫자를 문자열로 변환


거꾸로: 3진수 → 10진수로 변환

```java
String ternary = "101";
int decimal = Integer.parseInt(ternary, 3);
System.out.println(decimal);  // 출력: 10
```
