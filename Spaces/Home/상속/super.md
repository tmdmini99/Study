super - 
자식 클래스는 부모클래스를 [[상속]]받았기 때문에 자유롭게 부모의 모든 프로퍼티를 사용할 수 있다. 하지만 그럼에도 자식과 부모사이의 구분이 있어야한다. 자식클래스가 부모클래스의 프로퍼티와 동일한 이름을 갖고 있다면 그것을 부모로부터 구분해 낼 수 있어야한다는 것이다.
```java
class Parent { int a = 10; }

class Child extends Parent {

    void display() {

        System.out.println(a);

        System.out.println(this.a);

        System.out.println(super.a);

    }

}

public class Inheritance02 {

    public static void main(String[] args) {

        Child ch = new Child();

        ch.display();

    }

}
```

super() -

this() 메소드가 같은 클래스의 다른 생성자를 호출할 때 사용된다면, super() 메소드는 부모 클래스의 생성자를 호출할 때 사용됩니다.
자식 클래스의 인스턴스를 생성하면, 해당 인스턴스에는 자식 클래스의 고유 멤버뿐만 아니라 부모 클래스의 모든 멤버까지도 포함되어 있습니다.

따라서 부모 클래스의 멤버를 초기화하기 위해서는 자식 클래스의 생성자에서 부모 클래스의 생성자까지 호출해야만 합니다.

이러한 부모 클래스의 생성자 호출은 모든 클래스의 부모 클래스인 Object 클래스의 생성자까지 계속 거슬러 올라가며 수행됩니다.

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

**①**      //super(40);

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