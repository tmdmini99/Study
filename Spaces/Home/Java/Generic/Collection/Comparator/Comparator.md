


- `Comparator<EverspinVo>`는 `EverspinVo` 객체 두 개를 비교해서 어느 쪽이 더 크거나 작은지 판단하는 역할을 해요.
    

```java
Comparator<EverspinVo> comp = Comparator.comparing(EverspinVo::getTimestamp);
```

이 코드는 `EverspinVo` 객체들을 `getTimestamp()` 값 기준으로 오름차순 정렬하는 비교기준을 만든 거예요.

