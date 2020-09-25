---
title: 'Algorithm \| 완전 탐색: 소수 찾기'
excerpt: '프로그래머스의 \<소수 찾기\> 라는 문제를 통해 소수 여부를 판단하는 코드와 permutation을 구하는 코드를 알아보겠습니다.'
date: 2020-08-25
last_modified_at: 2020-09-25T16:22:22

category:
  - Algorithm

tags:
  - Algorithm
  - JavaScript
  - JS
  - ECMAScript
  - ES6
  - permutation
  - prime number
---

## 문제: 소수 찾기
[문제 자세히 보기](https://programmers.co.kr/learn/courses/30/lessons/42839)

프로그래머스 level 2의 **소수 찾기** 라는 문제입니다.

* 주어진 수가 소수(素數, prime number)인지 아닌지 찾는 문제
* 주어진 문자들의 순서를 바꿔 모든 조합을 찾는 문제

이 둘을 모두 해결할 줄 알아야 풀 수 있습니다.

C++의 경우엔 `next_permutation` 이나 `prev_permutation`이 있어서 permutation에 크게 고민하지 않아도 되지만, JS는 그렇지 않아서 필요할 때마다 일일이 만들어야 한다는 아쉬움이 있습니다.

그래서 이번 기회에 소수 구하는 법과 permutation에 대해서 정리해 보려고 합니다.



## isPrime
다음 코드는 `num`이 소수인지 아닌지 판단합니다.

```js
function isPrime(num) {
  if(num < 2) return false;
  if(num === 2 || num === 3) return true;

  for(let i = 2; i <= Math.sqrt(num); ++i) {
    if(num % i === 0) return false;
  }
  return true;
}
```

## Permutation
다음 코드는 배열을 permutation 해줍니다.

```js
function permutation(n, m) {
  if(m <= 0) return [[]];
  if(m === 1) return n.map(e => [e]);

  let ret = [];
  for(let i = 0; i < n.length; ++i) {
    ret = [...ret, ...permutation([...n.slice(0, i), ...n.slice(i + 1)], m - 1).map(e => [n[i], ...e])];
  }
  return ret;
}
```
<br>
다음은 문자열에 대해서 permutation을 하는 코드입니다.

```js
function permutation(s, n) {
    if(n <= 0) return [""];
    let ret = [];
    for(let i = 0; i < s.length; ++i) {
        ret = [...ret, ...permutation(s.slice(0, i) + s.slice(i + 1), n - 1).map(e => s[i] + e)];
    }
    return ret;
}
```
## Combination
문제와는 직접적인 연관은 없지만 그래도 그냥 지나치면 섭하니 combination을 구하는 코드에 대해서도 알아보겠습니다.

만약 combination을 하고 싶다면 재귀 호출할 때 전달하는 파라메터를 조금 수정해주면 됩니다.

```js
function combination(n, m) {
  if(m <= 0) return [[]];
  if(m === 1) return n.map(e => [e]);

  let ret = [];
  for(let i = 0; i < n.length; ++i) {
    // [...n.slice(0, i), ...n.slice(i + 1)] -> n.slice(i + 1)
    ret = [...ret, ...combination(n.slice(i + 1), m - 1).map(e => [n[i], ...e])];
  }
  return ret;
}
```

### 모든 부분 집합 구하기
추가로, `combination(n, 0)`, `combination(n, 1)`, ... `combination(n, n)`을 구하는 방법입니다.

쉽게 말해 배열의 모든 부분 집합을 구하는 방법입니다.
우선 위에서 정의한 `combination()` 함수를 이용하여 구해보겠습니다.

```js
let arr = [1, 2, 3, 4, 5];
let subset = [];
for(let i = 0; i <= arr.length; ++i) {
  subset.push(...combination(arr, i));
}
```

이 외에도 재귀 함수가 아닌 반복문을 사용하여 더 간단하게 구하는 방법도 있습니다.

```js
let arr = [1, 2, 3, 4, 5];
let subset = [[]];
arr.forEach(n => {
  subset.forEach(s => {
    subset.push([...s, n]);
  });
});
```