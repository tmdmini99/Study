


어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

|   |   |
|---|---|
|입력값 〉|[6, 5, 5, 5, 3, 2, 1, 0]|
|기댓값 〉|4|
```java
import java.util.*;
class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        Arrays.sort(citations);
        for(int i=0; i<citations.length; i++){
            if(citations[i] >= citations.length-i){
                answer = citations.length-i;
                break;
            }
        }
        return answer;
    }
}
```

4번째에서 `citations[i]` 값이 5이고 이 값이 `citations.length-i` 즉 8 - 4 보다 크기때문에 이 값이 정답.