---
title: 'Algorithm \| Rotate Array'
date: 2020-09-20
last_modified_at: 2020-09-20T16:50:13

category:
  - Algorithm

tags:
  - Algorithm
  - JavaScript
  - JS
  - ECMAScript
  - ES6
  - leetCode
  - Top Interview Questions Easy Collenction
---

> 본 글은 leetCode의 [Rotate Array](https://leetcode.com/problems/rotate-array/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Pop & Unshift
제가 해결한 방법입니다. `Array.prototype.pop()`으로 배열의 맨 마지막 값을 pop하고 그 값을 `Array.prototype.unshift()`로 맨 앞에 넣어줍니다.

```js
var rotate = function(nums, k) {
  while(k--) nums.unshift(nums.pop());
};
```



## Approach #3: Using Cyclic Replacements
![rotate array](/assets/images/2020-09-19-rotate-array/189_Rotate_Array.png)

위 사진과 같은 성질을 이용하여 배열을 회전시킵니다.

```js
var rotate = function(nums, k) {
  k %= nums.length;

  for(let moves = 0, start = 0; moves < nums.length; ++start) {
    let cur = start;
    let tmp = nums[cur];
    do {
      cur = (cur + k) % nums.length;
      [tmp, nums[cur]] = [nums[cur], tmp];
      moves++;
    } while(cur !== start);
  }
};
```

### 시간복잡도
O(n)



## Approach #4: Using Reverse
배열의 순서를 뒤집어 회전한 결과를 얻을 수 있습니다.

```
original list    : [ 1, 2, 3, 4, 5, 6, 7 ]
reverse 0 to 6   : [ 7, 6, 5, 4, 3, 2, 1 ]
reverse 0 to 2   : [ 5, 6, 7, 4, 3, 2, 1 ]
reverse 3 to 6   : [ 5, 6, 7, 1, 2, 3, 4 ]
```

단, JS의 `Array.prototype.reverse()`는 배열의 부분을 뒤집을 수 없기 때문에 부분을 뒤집을 수 있는 새로운 함수를 정의하여 해결합니다.

```js
var rotate = function(nums, k) {
  k %= nums.length;

  const reverse = (array, start, end) => {
    while(start < end) {
      [array[start], array[end]] = [array[end], array[start]];
      start++;
      end--;
    }
  }

  reverse(nums, 0, nums.length - 1);
  reverse(nums, 0, k - 1);
  reverse(nums, k, nums.length - 1);
};
```

### 시간복잡도
O(n)



## References
> [https://leetcode.com/problems/rotate-array/solution/](https://leetcode.com/problems/rotate-array/solution/)