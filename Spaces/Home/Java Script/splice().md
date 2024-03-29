

## .splice()

자바스크립트의 `splice()` 메서드는 배열 객체에 사용할 수 있는 내장 메서드입니다. 이는 기존 요소를 삭제하거나 교체하여 배열의 내용을 변경하며, 제거된 요소가 담긴 별도의 배열을 새로 반환합니다.
 `splice()` 메서드로 배열 요소 삭제하기

다음과 같은 내용이 담긴 `months`라는 배열이 있다고 가정해보겠습니다. 배열 이름은 달, 월을 뜻하는 `months`지만 요일이 배열에 섞여있는 것을 확인할 수 있습니다.


```js
// 월과 요일명이 혼합된 배열
let months = ["January", "February", "Monday", "Tuesday"];
```

```js
// 요일이 담긴 배열 생성하기 
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2); // 인덱스 2부터 배열 변경

console.log(days); // ["Monday", "Tuesday"]
```


`splice()` 메서드를 사용할 때는 `start` 매개변수에 꼭 값을 전달해야 하는데, 이 매개변수는 배열의 변경을 시작할 인덱스를 지정합니다. 위의 예시에서 인수로 전달된 `2`는 `splice()` 메서드가 인덱스 `2`에서부터 요소를 제거하기 시작할 것이라는 뜻입니다.

`deleteCount`라는 두 번째 매개변수에 값을 전달하면 배열에서 제거할 요소의 수를 지정할 수도 있습니다. 예를 들어, 하나의 요소만 제거하기 위해 다음과 같이 숫자 `1`을 두 번째 인수로 전달할 수 있습니다. 



`splice()` 메서드를 사용하여 `months` 메서드에서 요일 이름을 제거하는 동시에 요일만 포함된 새 배열을 만들 수 있습니다. 이렇게 말이죠.

```js
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2, 1); // 요소 하나만 삭제

console.log(days); // ["Monday"]
console.log(months); // ["January", "February", "Tuesday"]
```

`deleteCount` 매개변수를 생략하면 `splice()`는 지정된 `start` 인덱스부터 배열의 끝까지의 모든 요소를 제거합니다.

## `splice()`를 사용해 배열 요소 제거하고 새로 추가하기

`splice()` 메서드로 요소를 삭제하는 동시에 바로 새 요소를 추가할 수도 있습니다. `deleteCount` 매개변수 뒤에 배열에 추가할 요소를 전달하기만 하면 됩니다.

필수 매개변수와 선택적 매개변수를 모두 전달하는 경우의 문법은 다음과 같습니다.

```js
// 필수 매개변수와 선택적 매개변수가 포함된 splice() 문법 
Array.splice(start, deleteCount, newItem, newItem, newItem, ...)
```

다음 예시에서는 `months` 배열에 "March"와 "April"을 추가하면서 "Monday"와 "Tuesday"를 제거하는 방법을 보여줍니다.

```js
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2, 2, "March", "April"); // 요소 두 개를 제외하고, 다른 요소를 추가

console.log(days); // ["Monday", "Tuesday"]
console.log(months); // ["January", "February", "March", "April"]
```

## 요소를 제거하지 않고 새 배열 요소 추가하기

마지막으로 숫자 `0`을 `deleteCount` 매개변수에 전달하여 기존 요소를 삭제하지 않고도 새 요소를 추가할 수 있습니다. 요소가 제거되지 않을 경우 빈 배열이 반환되며, 경우에 따라 반환된 빈 배열을 변수에 저장해서 사용할 수 있습니다.

다음 예시에서는 기존 요소를 삭제하지 않고 "February" 요소 옆에 새 요소 "March"를 추가하는 것을 보여줍니다. 앞서 언급한 것처럼 반환되는 값이 빈 배열이므로 이를 별도로 저장하지 않아도 됩니다.

```js
let months = ["January", "February", "Monday", "Tuesday"];
months.splice(2, 0, "March"); // 두 번째 매개변수가 0이므로 기존 요소 모두 유지

console.log(months); 
// ["January", "February", "March", "Monday", "Tuesday"]
```

---
