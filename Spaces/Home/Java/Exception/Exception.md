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
//------------------------------------------------------------------------
public void method2() throws ClassNotFoundException {
 Class clazz = Class.forName("java.lang.String22");
}
```


예외 처리는 귀찮은 일이다. 그래서 예외를 다음 사용자에게 전가(throws)하거나 try...catch로 감싸고 아무것도 하지 않고 싶은 유혹에 빠지기 쉽다. 하지만 예외는 API를 사용하면서 발생할 수 있는 잠재적 위협에 대한 API 개발자의 강력한 암시다. 이 암시를 무시해서는 안 된다. 물론 더욱 고민스러운 것은 예외 처리 방법에 정답이 없다는 것이겠지만 말이다.



### **throw(예외 발생 시키기)**

throw는 **개발자가 직접 예외를 발생시키고싶을 때 쓰는 것**이다.
주로 `RuntimeException(UnCheckedException)` 처리를 위해 쓰는 방식이다.

**throw는 Exception을 던질 때 예외 내용도 던져주지 않는다.**
그래서 개발자가 직접 Exception을 따로 커스터마이징해서 만들고 그 안에 메시지를 넣어서 던져주는 방식이다.
throw는 개발자가 직접 예외를 발생시키고싶을 때 쓰는 것이지만 **추가적으로 Exception 재정의**해서 사용하기도 한다. (**CustomException**)

| 예외                      | 사용해야 할 상황                            |
| ------------------------- | ------------------------------------------- |
| IllegalArgumentException  | 매개변수가 의도하지 않은 상황을 유발시킬 때 |
| IllegalStateException     | 메소드를 호출하기 위한 상태가 아닐 때       |
| NullPointerException      | 매개 변수 값이 null 일 때                   |
| IndexOutOfBoundsException | 인덱스 매개 변수 값이 범위를 벗어날 때      |
| ArithmeticException       | 산술적인 연산에 오류가 있을 때              |
| IOException                          |      try...catch하거나 throw 해야 한다는 뜻                                       |

**ExceptionHandler** ?


사용법
```java
	throw new IOException;
	throw new ArithmeticException("두번째 인자값은 0이 될수 없습니다.");
	
```



예제 코드
```java
class  Test {  
    public void tests(String a, String b) throws NumberFormatException{  
        try {  
            int sum = Integer.parseInt(a) + Integer.parseInt(a);  
            System.out.println("문자 입력 합은 "+sum);  
        }catch (NumberFormatException e){  
            System.out.println("숫자형 문자 x");  
            throw e;  
        }  
    }  
}

public class Main {  
  
    public static void main(String[] ars) {  
  
        Test test = new Test();  
  
        try {  
            test.tests("d","d");  
  
        }catch (NumberFormatException e){  
            System.out.println("숫자 x");  
        }
}


```
### **사용자 정의 예외와 예외 발생**

사용자 정의 예외 클래스란 개발자가 직접 정의하여 만드는 예외를 말합니다. 일반 예외(Checked Exceptino)나 실행 예외(RuntimeException, UnChecked Exception) 중 하나로 만들 수 있습니다. 전자는 Exception을 상속하면 되고, 후자는 RuntimeException을 상속하면 됩니다.

```java
public class AnyCreateException extends Exception {
  public AnyCreateException() {};
  public AnyCreateException(String message) {
     super(message);
// 사용자 정의 예외의 생성. 생성자는 보통 두 가지를 선언하는데
// 두 번째의 경우 에외 메시지를 전달하기 위해 String 타입의 매개 변수를 갖는 생성자이다.
 }
}
.....
public class Account{
int balance = 100;
 public void withdraw(int money) throws AnyCreateException {
  if(balance < money){
   throw new AnyCreateException("Excpetion 발생!");
// AnyCreateException 의 두 번째 생성자를 선택한 것이다.
// 모든 예외는 Exception이 가지고 있는 메소드들을 호출 할 수 있다.
// 그 중 getMessage 메소드가 이 예외 메시지를 리턴한다. 사용은 아래에 구현했다.
 }
}
....
public static void main(String[] args){
 Account account = new Account();
 try{
 account.withdraw(30000);
}
 catch ( AnyCreateException e ) {
  String message = e.getMessage(); // 메시지를 담은 생성자를 선택했을 때, 그 메시지를
                              // getMessage() 메소드가 리턴한다.
  System.out.println(message);
  e.printStackTrace(); // 예외가 어디에서 발생했는지 출력해준다.
}
```



### finally
finally 구문은 예외처리가 발생여부를 떠나 무조건 실행하도록 하는 구문입니다.
```java
finally {
	System.out.println("배열");
}
 
```


| 단계      | 설명                   |
| ------- | -------------------- |
| try     | 예외 발생을 조사하는 문장 검사    |
| catch   | 예외가 발생했을 때 실행시킬 코드   |
| finally | 마지막에 반드시 실행시켜야 하는 코드 |



#### **catch 안에 쓸수 있는 코드(Exception Method)**

#### e.getMessage()
오류에 대한 기본적인 내용을 출력해준다. 상세하지 않다.

사용법
```java
	System.out.println(e.getMessage());
```

#### e.toString()
e.toString() 은 어떤 Exception이 발생하였으며, 원인이유를 보여준다.
하지만 에러의 발생위치는 보여주지않는다.
에러는 발생했지만 위 로그만 가지고는 에러의 위치를 찾기는 힘들다.
```java
System.out.println(e.toString());
```

#### e.printStackTrace()
메소드 getMessage, toString과는 다르게 printStackTrace는 리턴값이 없다. 
이 메소드를 호출하면 메소드가 내부적으로 예외 결과를 화면에 출력한다. 
printStackTrace는 가장 자세한 예외 정보를 제공한다.
```java
e.printStackTrace();
```

**log4에서는 e.printStackTrace()를 log 안에 담을수 없기 떄문에**
**log.error("error : ", e); 로 사용하면 된다.**


##### 예제


**getMessage()**
```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
        try{
        /* int로 형변환이 안되는 문자열을 넣어 강제로 Exception 발생 */
            String product = "사과";
            int productCnt = Integer.valueOf(product);
        }catch (Exception e){
            System.out.println(e.getMessage());
        }
    }
}
```

결과

![[Pasted image 20231218133627.png]]

-> For input string: "사과"
e.getMessage() 은 정말 Exception의 유형도 없이 정말 간단하게 왜 에러가 발생하였는지 보여주기만 합니다.

---
**e.toString()**
```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
        try{
            String product = "사과";
            int productCnt = Integer.valueOf(product);
        }catch (Exception e){
            System.out.println(e.toString());
        }
    }
}
```
![[Pasted image 20231218133755.png]]


---
**e.printStackTrace()**
```java
package joon;

public class codeTest {
    public static void main(String[] args) throws Exception{
        try{
            String product = "사과";
            int productCnt = Integer.valueOf(product);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

![[Pasted image 20231218133759.png]]

















## 예외의 종류
- Throwable
- Error
- Exception
- Runtime Exception
 ![[Pasted image 20231218172854.png]]

## Throwable

클래스 Throwable은 범 예외 클래스들의 공통된 조상이다. 모든 예외 클래스들이 가지고 있는 공통된 메소드를 정의하고 있다. 중요한 역할을 하는 클래스임에는 틀림없지만 이 클래스를 직접 사용하지는 않기 때문에 우리에게는 중요하지 않다.

## Error

에러는 여러분의 애플리케이션의 문제가 아니라 그 애플리케이션이 동작하는 가상머신에 문제가 생겼을 때 발생하는 예외다. 애플리케이션을 구동시키기에는 메모리가 부족한 경우가 이에 속한다. 이런 경우는 애플리케이션 개발자가 할 수 있는 것이 없다. 따라서 예외처리를 하지 말고 그냥 에러로 인해서 애플리케이션이 중단되도록 내버려둔다. 대신 자신의 애플리케이션이 메모리를 과도하게 사용하고 있다면 로직을 변경하거나 자바 가상머신에서 사용하는 메모리의 제한을 변경하는 등의 대응을 한다.

## Exception

결국 우리의 관심사는 Exception 클래스와 RuntimeException 클래스로 좁혀진다. 우선 Exception 클래스의 하위 클래스들의 목록을 살펴보자. 아래 클래스들은 모두 Exception 클래스를 상속한 예외 클래스다.

![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/2065.png)

필자가 강조 표시한 부분을 보자. 저 많은 클래스 중의 하나가 RuntimeException이다. 도대체 RuntimeException 클래스는 어떤 특이점이 있길래 부모 클래스인 Exception 클래스와 함께 언급되는 것일까? RuntimeException을 제외한 Exception 클래스의 하위 클래스들과 RuntimeException 클래스의 차이를 자바에서는 checked와 unckecked라고 부른다. 관계를 정리하면 아래와 같다.

- checked 예외 - RuntimeException을 제외한 Exception의 하위 클래스
- unchekced 예외 - RuntimeException의 하위 클래스

![[Pasted image 20231220111541.png]]
#### 1) Checked Exception

체크 예외는 예외를 잡아서 처리하거나(`try ~ catch`), 이게 안되면, 예외를 밖으로 던지는 `throw 예외`를 필수로 선언해야 한다.  
그렇지 않으면 컴파일 오류가 발생한다. 이것 때문에 장점과 단점이 동시에 존재한다.

- 장점 : 개발자가 실수로 예외를 누락하지 않도록 `컴파일러를 통해 문제를 잡아주는` `훌륭한 안전 장치`이다.
    
- 단점 : 하지만 실제로는 개발자가 모든 체크 예외를 반드시 잡거나 던지도록 처리해야 하기 때문에, `너무 번거로운 일`이 된다. 크게 신경쓰고 싶지 않은 예외까지 모두 챙겨야 한다. 특히 `의존관계`가 생긴다!
    
- 종류
    

```java
* IOException : 파일, 소켓 등에 대한 읽기 또는 쓰기와 같이 잘못될 수 있는 입출력 작업에 대한 일반적인 예외
* SQLException : 데이터베이스 관련 예외를 처리하는 데 사용
* ClassNotFoundException : 일반적으로 Java의 리플렉션 메커니즘을 사용할 때 런타임에 클래스를 찾을 수 없을 때 발생
```

#### 2) RuntimeException(Unchecked Exception)

**런타임 예외**란 프로그래밍 소스코드를 작성하여 컴파일과정에서는 문제를 발견하지 못하고 정상적으로 컴파일이 진행되었으나, 프로그램을 실행중에 발생하는 오류사항들을 대체로 예외라고 정의한다. Unchecked Exception은 이 `런타임 예외`를 상속한다.

Unchecked Exception은 `복구 불가능한 예외`이다.  
=> 예외 발생시 `런타임 중지! 즉, 프로그램이 종료가 된다는 뜻`이다!  
(**Unchecked Exception**)

ex) 범위를 넘어선 배열접근, 정수를 0으로 나누는 경우(ArithmeticException), Null포인터 오류등이 있다.

**그럼 이러한 오류가 발생하면 개발자는 어떻게 처리를 해주어야 하느냐?**

> **결론적으로 말하자면**, 이 `Exception에 대한 에러 메시지를 출력`하는 것이다.  
>   
>   
> 사실, `Checked Exception을 만나면` `더 구체적인 Unchecked Exception`을 발생시켜 **정확한 정보를 전달하고 로직의 흐름을 끊는 것이 좋다**. ex. SQLException  
> Spring이나 JPA 등에서 SQLException을 처리하지 않는 이유도 적절한 RuntimeException으로 던져주고 있기 때문이다.

- 종류

```java
* NullPointerException : null 참조를 사용하여 객체의 메서드나 필드에 액세스하려고 하면 발생
* ArithmeticException : 산술 연산(예: 0으로 나누기)으로 인해 오류가 발생
* ArrayIndexOutOfBoundsException : 잘못된 인덱스가 있는 배열 요소에 액세스하려고 하면 발생
* IllegalArgumentException : 잘못된 인수가 메소드에 전달되면 발생
* InvalidFormatException : 데이터 형식이 유효하지 않은 상황에 직면하고 이 문제를 나타내기 위해 예외를 발생
* NumberFormatException : 문자열을 숫자로 변환하려고 시도했지만 형식이 유효하지 않은 경우 발생
...
```


#### 1) CheckedException 처리전략

- 기본원칙은 `언체크(런타임) 예외`를 사용하자.
    
- CheckedException는 비즈니스 로직상 `의도적으로` 던지는 예외로만(try ~ catch & throws) 사용하자.
    
- throws Exception 사용 x => 구체적인 체크 예외로 밖으로 던지는 방식(try ~ catch & throws)으로 하자.  
    `SQLException`,`ConnectionException` 같은 시스템 예외는 Controller, Service 클래스에서는 대부분 복구가 불가능하고 처리할 수 없는 체크 예외이다. 따라서 다음과 같이 처리해주어야 한다.
    

```java
void method() thorws SQLException, ConnectionException {...}
```

#### 2) UnCheckedException 처리전략

- 런타임 예외를 사용하면 중간에 기술이 변경되어도 해당 예외를 사용하지 않는 controller, service에서는 `코드를 변경하지 않아도 된다.`
- 그래서 스프링에서도 `기본적으로 런타임 예외를 제공`한다.

```java
throw new ArithmeticException("Division by zero");
```

- 예외를 `공통으로 처리`하는 `ExceptionHandler`를 만들어서 처리하자.



checked 예외는 반드시 예외처리를 해야 하는 되는 것이고, unchekced는 해도 되고 안 해도 되는 예외다. 바로 이 지점이 IOException과 ArithmeticException의 차이점이다. 아래는 두개 클래스들의 가계도를 보여준다.



![](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/516/2093.png)

강조 표시한 부분을 주의 깊게 살펴보자. ArithmeticException의 부모 중에 RuntimeException이 있다. 반면에 IOException은 Exception의 자식이지만 RuntimeException의 자식은 아니다. 이런 이유로 IOException은 checked이고 ArithmeticException은 unchekced이다. (Error도 unchecked이다)



#### 나만의 예외 만들기
```java 
package org.opentutorials.javatutorials.exception;
class DivideException extends RuntimeException {

DivideException(){

super();

}

DivideException(String message){

super(message);

}

}

class Calculator{

int left, right;

public void setOprands(int left, int right){

this.left = left;`

this.right = right;

}

public void divide(){

if(this.right == 0){

throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");

}

System.out.print(this.left/this.right);

}

}

public class CalculatorDemo {

public static void main(String[] args) {

Calculator c1 = new Calculator();

c1.setOprands(10, 0);

c1.divide();

}

}
```




```java
package org.opentutorials.javatutorials.exception;
class DivideException extends Exception {

DivideException(){

super();

}

DivideException(String message){

super(message);

}

}

class Calculator{

int left, right;

public void setOprands(int left, int right){

this.left = left;`

this.right = right;

}

public void divide(){

if(this.right == 0){
	try{
		throw new DivideException("0으로 나누는 것은 허용되지 않습니다.");
	}catch(DivideException e){
		e.printStackTrace();
	}



}

System.out.print(this.left/this.right);

}

}

public class CalculatorDemo {

public` `static void main(String[] args) {

Calculator c1 = new Calculator();

c1.setOprands(10, 0);

c1.divide();

}

}
```




# 참조 사이트
---


참조 - https://chanhuiseok.github.io/posts/java-3/

https://opentutorials.org/course/1223/6226
https://opentutorials.org/course/1223/6227
https://opentutorials.org/course/1223/6228

https://lnsideout.tistory.com/entry/JAVA-etoString-egetMessage-eprintStackTrace-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC
https://velog.io/@mooh2jj/%EC%9E%90%EB%B0%94-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%ACtry-catch-throw-throws