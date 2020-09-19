---
title: 'Algorithm \| Number of 1 Bits'
date: 2020-09-19
last_modified_at: 2020-09-19T21:10:16

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
  - Number of 1 Bits
---

> 본 글은 leetCode의 [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Approach #1: Loop and Flip
입력된 정수 n을 binary code로 표기한다면 int형 데이터의 크기는 4 byte이므로 32자리의 수가 될 것입니다.

반복문을 통해 각 자리를 순회하면서 값이 1인지 판단해 1이 나온 횟수를 도출합니다.

```js
var hammingWeight = function(n) {
  let bits = 0;
  let mask = 1;
  for(let i = 0; i < 32; ++i) {
    bits += n & mask;
    n >>= 1;
  }
  return bits;
};
```

### 시간복잡도
**O(1)**

입력값 n이 몇이든 상관없이 자리수 비교 32번을 마치면 답이 구해집니다.



## Approach #2: Bit Manipulation Trick
`n & (n-1)`의 값이 n과 비교해 가장 마지막 자리의 1이 없다는 성질을 이용한 방법입니다.

예를 들어, `12`와 `12 - 1 = 11`을 `AND` 연산한 결과는 다음과 같습니다.

| n | n - 1 | n & (n - 1) |
|-----|-----|-----|
| 12 | 11 | 8 |
| 1100<sub>2</sub> | 1011<sub>2</sub> | 1000<sub>2</sub> |

이렇게 `n & (n - 1)`을 한번 할 때마다 가장 작은 1이 하나씩 없어지는 연산을 반복하여 1의 갯수를 구합니다.

```js
var hammingWeight = function(n) {
  let ret = 0;
  while(n) {
    n &= n - 1;
    ret++;
  }
  return ret;
};
```

### 시간복잡도
**O(1)**

`while`문의 반복 횟수는 n의 1 bit 개수에 따라 다릅니다. 1 bit가 하나도 없다면 반복 횟수는 0이 되고, 그때가 *best case* 일 것입니다. 반대로 *worst case* 에선 모든 bit의 값이 1이라면 반복 횟수는 32가 됩니다.(4 byte 기준) 하지만 *best case* 와 *worst case* 모두 실행 횟수는 n의 값과 상관없는 상수 함수이기 때문에 **시간복잡도는 O(1)**이 됩니다.