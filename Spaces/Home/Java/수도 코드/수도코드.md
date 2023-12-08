Pseudo Code 수도 코드란 의사(가짜) 코드라는 의미를 가진,  
프로그램이나 알고리즘이 수행해야 할 내용을 **언어**로 간략하게 서술한 것을 말한다.  
여기서 **언어**란 자바스크립트나 파이썬 같은 프로그래밍 언어가 아니고,  
**실제 세계에서 사용하는 사람의 언어`**`를 말한다.

## ✍️ 수도 코드를 쓰는 이유
- 실제 코딩하기 전 사고를 더 명확하게 할 수 있다.
- 코드의 검토와 수정이 더 용이하다.
- 프로그램 또는 알고리즘이 어떻게 실행되어야 하는지 쉽게 파악할 수 있다.
- 미리 오류를 수정할 수 있기 때문에 경제적이다.
- **문제 해결을 위한 도구로,  
    다른 사람과 프로그램의 흐름에 대해 소통하기 쉽게 도와준다**

### 수도 코드의 주요 구성 CONSTRUCTS
- SEQUENCE
- WHILE
- REPEAT-UNTIL
- FOR
- IF-THEN-ELSE
- CASE
- EXTRA CONSTRUCTS
    - CALL
    - EXCEPTION


### 실제 수도 코드에서 많이 쓰이는 키워드
- 입력 Input : READ, OBTAIN, GET
- 출력 Output : PRINT, DISPLAY, SHOW
- 계산 Compute : COMPUTE, CALCULATE, DETERMINE
- 초기화 Initialize : SET, INIT
- 요소를 추가 Add one : INCREMENT, BUMP
- 선형 증가 Linear progression : SEQUENCE
- 반복문 : WHILE, FOR
- 조건문 : IF-THEN-ELSE
- 마지막에 조건문이 있는 반복문 : REPEAT-UNTIL
- 대신 조건 분기 처리 IF-THEN-ELSE : CASE
- 불 Bool : TRUE / FALSE
- 그 외 : REPEAT-UNTIL RETURN BEGIN / [[Exception|EXCEPTION]] / END

### 수도 코드 예제
```java
// 1. 만약 나이가 60살 이상이면
// 2. 'passed'를 출력하고
// 3. 아니면
// 4. 'failed'를 출력한다.
```

```java
if (age <= 60) { // 만약 age(나이)가 60 이상이면
  print('passed') // 'passed'를 출력하고
} else { // 아니면
  print('failed') // 'failed'를 출력한다.
}
```





참조 - **https://velog.io/@strawberrycream/pseudocode**
