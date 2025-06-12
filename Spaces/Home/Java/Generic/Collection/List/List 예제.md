



```java
List<Integer> list = Arrays.asList(10, 20, 30, 40, 50, 60, 70);

// 2번째(index 1)부터 5번째(index 4)까지 자르기
List<Integer> sliced = list.subList(1, 5);  // [20, 30, 40, 50]

System.out.println(sliced);

```


`subList(fromIndex, toIndex)`는 **fromIndex 이상, toIndex 미만** 구간을 반환합니다.

