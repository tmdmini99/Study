프로그램 동작을 잠시 멈추기 위해 sleep 사용하는 방법에 대해 알아보겠다.  보통 CPU 점유을 다른 다른 프로세스나 스래드로 넘기기 위해 sleep을 사용 한다.

**sleep 사용 예제**

---

  

sleep 사용예는 아래와 같다.

  

|   |
|---|
|public class Sleep {<br><br>	public static void main(String[] args ) {<br><br>		System.out.println("sleep 실행 전");<br><br>		try {<br><br>			**Thread.sleep(1000);** //1초 대기<br><br>		} catch (InterruptedException e) {<br><br>			e.printStackTrace();<br><br>		}<br><br>		System.out.println("sleep 실행 후");<br><br>	}<br><br>}|

  

1000이면 1초다. sleep 인자 값에 적당한 시간 값을 넣어 주면 된다.

  

|   |
|---|
|try {<br><br>	**Thread.sleep(1000); //1초 대기**<br><br>   } catch (InterruptedException e) {<br><br>	e.printStackTrace();<br><br>   }|


---
참조 - https://jink1982.tistory.com/185