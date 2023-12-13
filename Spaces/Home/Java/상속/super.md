super -  **super**는 자신이 상속받은 부모 클래스에 대한 레퍼런스 변수로, **부모 클래스의 멤버에 접근할 때 사용**됩니다. 
주로 객체안에 있는 부모의 멤버변수와 자신의 멤버변수를 구별하기 위해 사용됩니다

자식 클래스는 부모클래스를 [[상속]]받았기 때문에 자유롭게 부모의 모든 프로퍼티를 사용할 수 있다. 하지만 그럼에도 자식과 부모사이의 구분이 있어야한다. 자식클래스가 부모클래스의 프로퍼티와 동일한 이름을 갖고 있다면 그것을 부모로부터 구분해 낼 수 있어야한다는 것이다.



```java
public class Book {

	String title ="미입력";
	int price = -1;
	int code = 100;
		
	public Book() {  	}                     // 기본생성자
		
	public Book(String title, int price) {      // 매개변수 2개인 생성자
		this.title = title;
		this.price = price;
	}

	
	public void showPrice() {
		System.out.println(title + "의 가격은 " + price + "원 입니다");
	}
}



public class EnglishBook extends Book {

	int code = 200;
	
	public EnglishBook() {    }                       // 기본생성자

	public EnglishBook(String title, int price) {    // 매개변수 2개인 생성자
		super(title, price);     
	}

	
	public void showPrice() {

		super.showPrice();      // 부모클래스의 메소드 호출
		
		System.out.println("");
		System.out.println("code       : " + code);
		System.out.println("this.code  : " + this.code);
		System.out.println("super.code : " + super.code);
		
		System.out.println("");
		System.out.println("price       : " + price);
		System.out.println("this.price  : " + this.price);
		System.out.println("super.price : " + super.price);
	
	}
}
```




super() - super( )는 자식 클래스의 생성자에서 부모 클래스의 생성자를 호출하기 위해서 사용됩니다. 


this() 메소드가 같은 클래스의 다른 생성자를 호출할 때 사용된다면, super() 메소드는 부모 클래스의 생성자를 호출할 때 사용됩니다.
- super( )는 생성자 코드안에서 사용 될 때, 다른 코드에 앞서 <span style="background:#fff88f">첫줄</span>에 사용되어야 합니다. 
- 자식 클래스의 모든 생성자는 <span style="background:#fff88f">부모 클래스의 생성자</span>를 포함하고 있어야 합니다. 그런데 만약 자식 클래스의 생성자에 부모 클래스의 생성자가 지정되어 있지 않다면, 컴파일러가 자동으로 부모 클래스의 기본생성자를 호출합니다. (이러한 경우, 부모클래스에 매개변수가 있는 생성자만 있고, 기본생성자가 없어 기본생성자를 호출할수 없다면 에러가 발생합니다.)

추상 클래스와 인터페이스 모두 반드시 오버라이딩을 해야함
```java
class Parent {

    int a;

    Parent() { a = 10; }

    Parent(int n) { a = n; }

}

class Child extends Parent {

    int b;

    Child() {

		super(40); //생성자 생성시 부모클래스의 생성자를 가져옴

        b = 20;

    }

    void display() {

        System.out.println(a);

        System.out.println(b);

    }

}

public class Inheritance04 {

    public static void main(String[] args) {

        Child ch = new Child();

        ch.display();

    }

}
```




https://velog.io/@rhdmstj17/java.-super%EC%99%80-super-%EC%99%84%EB%B2%BD%ED%95%98%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0   : super

https://tcpschool.com/java/java_inheritance_super

https://kadosholy.tistory.com/93

