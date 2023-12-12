자바에서 Object 클래스는 모든 클래스의 부모 클래스가 되는 클래스입니다.

따라서 자바의 모든 클래스는 자동으로 Object 클래스의 모든 필드와 메소드를 상속받게 됩니다.

즉, 자바의 모든 클래스는 별도로 extends 키워드를 사용하여 Object 클래스의 상속을 명시하지 않아도 Object 클래스의 모든 멤버를 자유롭게 사용할 수 있습니다.

자바의 모든 객체에서 toString()이나 clone()과 같은 메소드를 바로 사용할 수 있는 이유가 해당 메소드들이 Object 클래스의 메소드이기 때문입니다.



|메소드|설명|
|---|---|
|protected Object clone()|해당 객체의 복제본을 생성하여 반환함.|
|boolean equals(Object obj)|해당 객체와 전달받은 객체가 같은지 여부를 반환함.|
|protected void finalize()|해당 객체를 더는 아무도 참조하지 않아 가비지 컬렉터가 객체의 리소스를 정리하기 위해 호출함.|
|Class<T> getClass()|해당 객체의 클래스 타입을 반환함.|
|int hashCode()|해당 객체의 해시 코드값을 반환함.|
|void notify()|해당 객체의 대기(wait)하고 있는 하나의 스레드를 다시 실행할 때 호출함.|
|void notifyAll()|해당 객체의 대기(wait)하고 있는 모든 스레드를 다시 실행할 때 호출함.|
|String toString()|해당 객체의 정보를 문자열로 반환함.|
|void wait()|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|
|void wait(long timeout)|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지날 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|
|void wait(long timeout, int nanos)|해당 객체의 다른 스레드가 notify()나 notifyAll() 메소드를 실행하거나 전달받은 시간이 지나거나 다른 스레드가 현재 스레드를 인터럽트(interrupt) 할 때까지 현재 스레드를 일시적으로 대기(wait)시킬 때 호출함.|




"Object"클래스가 가진 메소드 중 "toString"메소드가 있습니다.

물론 "Object" 클래스의 모든 메소드는 모든 클래스가 사용이 가능합니다.

"**toString**" 메서드는 객체가 가지고 있는 정보나 값들을 문자열로 만들어 리턴하는 메소드 입니다.

평소에는 .toString()을 생략함 








참조 - 
