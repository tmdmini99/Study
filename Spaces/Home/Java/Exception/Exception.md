---
ddd: []
---
# **자바(JAVA) - 예외 처리(Exception Handling)**

---

자바에서 예외(exception) 란 사용자의 잘못된 조작이나 개발자의 코딩 실수로 인해 발생하는 프로그램 오류를 말합니다. 예외가 발생되면 프로그램은 곧바로 종료된다는 점에서 에러와 동일하나, 예외는 예외 처리를 통해 프로그램을 종료하지 않고 정상 실행 상태가 유지되도록 할 수 있습니다.


**모든 예외 클래스는 java.lang.Exception 클래스를 상속받습니다.**

- Exception 클래스 자체는 **checked exception** 이다.
- Exception 클래스는 **Throwable 클래스의 자식 클래스**이다.
- Exception 클래스의 자식 클래스 중 **RuntimeException 클래스는 Unchecked** 이다.
- 그 외 checked exception이 있다.



### **실행 예외 (Unchecked Exception)의 종류**

#### **1-1. NullPointerException(java.lang.NullPointerException)** 
객체 참조가 없는 상태일 때 발생합니다. null 값을 갖는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을 때 발생합니다. 객체가 없는 상태에서 객체를 사용하려 했으니 당연히 예외가 발생합니다.

#### **1-2. ArrayIndexOutOfBoundsException** 
배열에서 인덱스 범위를 초과하여 사용할 때 발생합니다. `int[] arr = new int[3];` 이라 해 놓고 `arr[4]=5;` 같은 대입 연산을 시도할 때 발생할 수 있습니다.
#### **1-3. NumberFormatException** 
문자열로 되어 있는 데이터를 숫자로 변경하는 경우가 많은데, 문자열을 숫자로 변환하는 방법 중 가장 많이 사용되는 코드는 Integer.parseInt(String s) 메소드와 Double.parseDouble(String s) 메소드입니다. 그런데 아래 코드를 확인해 보겠습니다.
```java
.... String data1 = "100"; 
String data2 = "이건안돼요"; 
int val1 = Integer.parseInt(data1);
int val2 = Integer.parseInt(data2); //NumberFormatException 발생
```

#### **1-4. ClassCastException** 
허용되지 않는데 억지로 타입 변환을 시도할 경우 발생합니다. 추상클래스 Animal을 상속하는 Dog, Cat 클래스와 RemoteControl 인터페이스를 상속하는 Television, Audio 클래스가 있다고 가정해 보겠습니다. 그리고 예외가 발생하는 상황은 아래 코드와 같습니다.
```java
Animal animal = new Dog();
Dog dog = (Dog) animal; // 문제 없음

RemotControl ex1 = new Television();
Television ex2 = (Television) ex1; // 문제 없음

Animal animal = new Animal();
Dog dog = (Dog) animal; // ClassCastException 발생
```



### **예외 처리 코드** (try~catch~finally~…)

본격적으로 예외를 처리(handling) 하는 것에 대해 알아보겠습니다. 기본 형태는 아래와 같습니다.


```java
try{
///예외가 발생할 가능성이 있는 코드
.... ......... ;
.............. .. ; << 1. 여기서 예외가 발생했을 때
.............. .... ; << 2. 예외 발생한 곳 아래는 실행하지 않고,
}
catch(예외클래스 e) {
   예외 처리; << 3. catch문에서 예외처리를 한다.
}
finally {
///무슨 일이 있든 항상 실행
}

#### **- 다중 catch문**
.....
try{
// 예외 1 발생위치
// 예외 2 발생위치
}
catch(예외1을 잡는 곳){
}
catch(예외2를 잡는 곳){
}



```

발생하는 예외별로 다중 catch문을 구성할 수 있지만, catch 블록이 여러개여도 먼저 발생한 예외에 대하여 단 하나의 catch 블록만 실행됩니다. 그 이유는 위에서 예외 1이 발생했다면, 그 밑의 예외 2 발생 코드는 실행하지 않고 바로 예외 1을 catch하는 곳으로 이동하기 때문입니다.

그러므로 다중 catch 블록을 작성할 때에는 **상위 예외 클래스가 하위 예외 클래스보다 밑에 위치하게 작성**해야 합니다.
**try에서 예외가 발생했을 때에는 catch블록이 작성된 순서대로 위에서 아래대로 차례로 검사하는데**, 만일 상위 예외 클래스가 더 위에 있었다면 하위 예외 클래스를 실행하지 않게 됩니다. 즉, **하위 예외 클래스가 상위 예외 클래스를 상속했기 때문에, 상위 예외 클래스를 잡는 catch 블록이 실행**되게 됩니다.

이 말이 무슨 의미인지, 예를 들어 `FileNotFoundException` 예외가 발생했다고 가정해 보겠습니다.
```java
.....
try{
// FileNotFoundException 예외 발생
}
catch(Exception e){
    // FileNotFoundException은 Exception의 자식 클래스이므로 이 부분의 catch가 실행됨
}
catch(FileNotFoundException e){
    // 정작 FileNotFoundException을 처리하려는 이 catch 블록은 실행되지 않음
}
```

 FileNotFoundException`의 상위 예외 클래스인` Exception`을 처리하는 catch 블록이 더 위에 있기 때문에,` FileNotFoundException` 이 생겼을 때 처리하기 위한 두 번째 catch 블록은 실행되지 않습니다.

따라서 **`Exception`** 처럼 상위 예외 클래스를 처리하는 코드는 catch문 맨 마지막에 작성합니다.

#### **- 멀티 catch(catch( 예외 1 | 예외 3  e))**

자바 7부터 하나의 catch 블록에서 여러 개의 예외를 처리할 수 있는 기능이 추가되었습니다.
```java
try{
  // 예외 1 발생위치 (혹은 예외 3 발생위치)
  // 예외 2 발생위치
}
catch( 예외 1 | 예외 3  e) // | 는 or 로 보면 된다. 예외 1 혹은 예외 3이 발생하면
해당한 catch 블록에서 처리하게 된다.
{
  예외 처리
}
catch( 예외2 e)
{
  예외 처리
}
```

### **예외 떠넘기기(throws)**

메소드 내부에서 예외가 발생할 수 있는 코드를 작성할 때, try-catch 블록으로 예외 처리 하는 것이 기본이지만, 경우에 따라서는 **메소드를 호출한 곳으로 예외 처리를 떠넘길** 수도 있습니다. 이 때 사용하는 키워드가 `throws`입니다.

throws 키워드는 메소드 선언부 맨 끝에 작성하며, 메소드에서 try-catch를 통해 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할을 합니다. throws 뒤에는 떠넘길 에외 클래스를 쉼표로 여러개 구분해서 나열할 수 있습니다. 발생할 수 있는 예외별로 throws 뒤에 나열하는 것이 보통이나, Exception 만 작성해서 모든 예외를 간단히 떠넘길 수도 있습니다.
```java
public void method1(){
 try {
   method2(); // throws가 붙은 method2는 반드시 이렇게 try문 안에서 호출되어야 함.
   // method2가 떠넘긴 예외를 아래 catch문을 통해 처리해 주고 있다.
}
 catch (ClassNotFoundException e1) {
  System.out.println("클래스가 존재하지 않음.");
}

public void method2() throws ClassNotFoundException {
 Class clazz = Class.forName("java.lang.String22");
}
```



참조 - https://chanhuiseok.github.io/posts/java-3/
