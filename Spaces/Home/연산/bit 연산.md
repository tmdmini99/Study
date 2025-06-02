




```java
class TryHelloWorld {
    public int nextBigNumber(int n) {
        int postPattern = n & -n, smallPattern = ((n ^ (n + postPattern)) / postPattern) >> 2;
        return n + postPattern | smallPattern;
    }
    public static void main(String[] args) {
        int n = 78;
        System.out.println(new TryHelloWorld().nextBigNumber(n));
    }
}
```

```java
int postPattern = n & -n;
```

- `n & -n` 은 **가장 오른쪽에 있는 1비트만 남기는 비트 연산**입니다.
    
- 예: `n = 78 → 1001110`  
    `-n = -78 → 0110010`  
    `n & -n = 0000010 = 2`
    

이건 **n의 가장 낮은 1비트 자리**만 추출하는 역할입니다

### `postPattern = n & -n`

- `n = 1001110`
    
- `-n = 2의 보수 = 0110010`
    
- `n & -n = 0000010` → `postPattern = 2`
    

➡️ `postPattern = 2`는 **가장 오른쪽 1비트 위치**입니다

```java
int smallPattern = ((n ^ (n + postPattern)) / postPattern) >> 2;
```

- `n + postPattern` : 다음으로 작은 수를 만들기 위한 중간 단계
    
- `n ^ (n + postPattern)` : n과 그 다음 숫자 사이의 차이를 나타냅니다 (변화된 비트)
    
- 그것을 `postPattern`으로 나누고 `>> 2` 하는 과정은,  
    → 변화된 패턴을 다시 **오른쪽으로 정렬**시켜 작은 수를 유지하게 하는 역할입니다.
    

이 줄은 전체적으로 **바뀐 비트를 재정렬해서 새로운 수를 만드는 패턴**을 정리해 주는 것입니다.


```java
smallPattern = ((n ^ (n + postPattern)) / postPattern) >> 2;
```

#### 먼저 계산:

`n + postPattern = 78 + 2 = 80` → 이진수: `1010000`

#### 이제 XOR 연산

```markdown
n          = 1001110
n+postPat. = 1010000
--------------------
XOR        = 0011110

```

(같은 자리이면 0, 다르면 1)

나누기

```java
0011110 (30) / postPattern (2) = 15 → 01111
```

