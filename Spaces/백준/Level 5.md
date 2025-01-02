

## 문제

두 정수 A와 B를 입력받은 다음, A+B 또는 A-B를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 A와 B가 주어진다. (0 < A, B < 10)

## 출력

첫째 줄에 A+B를 출력한다.

```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();
        System.out.println(a+b);
        sc.close();
    }
}
```


A-B
```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();
        System.out.println(a-b);
        sc.close();
    }
}
```



### 문제에서 주어진 조건

문제에서 주어진 조건은 **조규현**과 **백승환**이 각자의 **터렛 위치에서 류재명**의 위치까지의 거리를 계산했다고 하셨습니다. 이 때, 각 사람의 **터렛 위치**는 원의 **중심**에 해당하고, **류재명**은 원 위의 점으로 존재합니다.

- 즉, 각 사람은 자신이 계산한 거리(`r1`, `r2`)를 기준으로 **류재명**이 있을 수 있는 위치를 구하는 문제입니다.

자신의 위치 -> 거리를 삥 두르면 원이 된다.

그래서 두 원의 중심 좌표와 반지름을 이용해, 두 원이 어떤 관계인지 판별합니다. 이는 주로 기하학 문제에서 원의 교점을 계산하거나 관계를 분석할 때 사용됩니다.