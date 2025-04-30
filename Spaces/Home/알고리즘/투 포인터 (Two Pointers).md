
### **투 포인터 (Two Pointers)**

정렬된 배열에서 양 끝에서 가운데로 포인터를 이동하며 조건을 찾는 방식

**예: 정렬된 배열에서 두 수의 합이 target**

```java
int left = 0, right = nums.length - 1;
while (left < right) {
    int sum = nums[left] + nums[right];
    if (sum == target) return new int[]{left, right};
    else if (sum < target) left++;
    else right--;
}
```


