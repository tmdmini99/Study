
### **이분 탐색 (Binary Search)**

정렬된 배열에서 값을 O(log n) 시간에 찾는 방식

**예: target이 배열에 있는지**


```java
int left = 0, right = nums.length - 1;
while (left <= right) {
    int mid = (left + right) / 2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) left = mid + 1;
    else right = mid - 1;
}
```

