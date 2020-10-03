---
title: 'Algorithm \| Burst Balloons'
date: 2020-10-03
last_modified_at: 2020-10-03T17:09:07

category:
  - Algorithm

tags:
  - Algorithm
  - JavaScript
  - JS
  - ECMAScript
  - ES6
  - leetCode
  - Top Interview Questions Hard Collenction
---

> 본 글은 leetCode의 [Burst Balloons](https://leetcode.com/problems/burst-balloons/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Brute Force
모든 경우의 수를 직접 다 구한 다음 대소비교를 하는 방법입니다.

길이가 `n`인 배열 `nums`에 대하여 `i = 0`부터 `i = n - 1` 까지의 원소를 하나씩 빼고 `nums[i - 1] * nums[i] * nums[i + 1]`을 계산합니다.

그 다음엔 `nums[i]`가 제외된 배열에서 `j = 0`부터 `j = n - 2`까지, 또 그 다음엔 `nums[i]`와 `nums[j]`가 제외된 배열에서 `k = 0`부터 `k = n - 3`까지 각각 자신과 자신의 앞뒤에 있는 원소의 곱을 계산하는 식입니다.

맨 첫 원소를 뺄 때는 `n - 1`번, 두 번째 원소를 뺄 때는 `n - 2`번, 세 번째 원소를 뺄 때는 `n - 3`번 반복하므로 해당 방법의 시간 복잡도는 **O(N!)** 입니다.

굉장히 비효율적이기 때문에 다른 방법을 통해 시간 복잡도를 개선해보겠습니다.



## Devide and Conquer
분할정복 알고리즘을 적용할 때 사용하는 가장 기초적인 방법은 배열의 중앙을 기준으로 나누는 것일 겁니다.

```cpp
int lo = 0;
int hi = arr.size();
int mid = (lo + hi) / 2;
```

혹은 정렬을 위한 partitioning을 할 때 pivot으로 partition의 처음과 마지막 값의 평균을 사용하기도 합니다.

```cpp
int lo = 0;
int hi = arr.size();
int pivot = (arr[lo] + arr[hi]) / 2;
```

하지만 이 문제의 경우엔 이런 식으로 배열을 나누는 방법은 적절치 않습니다.
부분의 합이 전체로 이어지지 않기 때문입니다.
선택된 원소가 차례대로 배열에서 빠지기 때문에 순서에 따라 배열은 달라지게 되고 이를 부분의 합으로 모두 추적할 수 없기에 이런 결과가 나오는 것입니다.

대신 다른 기준으로 배열을 분할합니다.
바로 **가장 마지막에 빠지는 원소**를 사용하는 것입니다.

예를 들어, `[1, 2, 3, 4, 5, 6, 7]`이란 배열의 maxCoins 값을 구한다고 하면 배열의 시작과 끝에 `1`을 추가한 `[1, 1, 2, 3, 4, 5, 6, 7, 1]` 배열을 사용하도록 전처리를 해줍니다.
이렇게 하면 `1, 2, ... , 7` 중 어느 것을 선택하던 곱은 `1 * nums[i] * 1`로 고정이 되고 `nums[i]` 기준으로 나뉜 원소들은 반대편에 있는 원소들의 곱셈에 영향을 미치지 못하기 때문에 분할의 기준점으로 사용할 수 있게 됩니다.

만약 `4`를 선택한다면 마지막 곱은 `1 * 4 * 1`이 되는 것이고 배열은 `[1, 1, 2, 3, 4] + [4, 5, 6, 7, 1]`로 나뉠 것입니다.

그리고 또다시 각각 `2`와 `6`을 마지막에 선택한다면 `[[1, 1, 2] + [2, 3, 4]] + [[4, 5, 6] + [6, 7, 1]] `와 같이 나뉘겠습니다.

물론, 모든 경우 중에서 최대값을 찾아야 하기 때문에 `int mid = (lo + hi) / 2`처럼 피벗을 따로 구하는 공식이 있는 것은 아니고 모든 원소들에 대해서 분할을 해본 뒤에 가장 큰 값을 선택합니다.
방금 전엔 분할이 어떤 식으로 이뤄지는지 보여주기 위해 `4`를 중심으로 쪼갰지만 사실은, 

```
[1, 1] + [1, 2, 3, 4, 5, 6, 7, 1]
[1, 1, 2] + [2, 3, 4, 5, 6, 7, 1]
[1, 1, 2, 3] + [3, 4, 5, 6, 7, 1]
[1, 1, 2, 3, 4] + [4, 5, 6, 7, 1]
[1, 1, 2, 3, 4, 5] + [5, 6, 7, 1]
[1, 1, 2, 3, 4, 5, 6] + [6, 7, 1]
[1, 1, 2, 3, 4, 5, 6, 7] + [7, 1]
```

를 모두 해본 뒤에 이 중에서 가장 최대값을 리턴하는 것입니다.
분할된 배열은 그 안에서 다시 분할 정복을 거치면서 말입니다.

이때 중복되는 부분 문제를 여러번 계산하지 않기 위해 캐시를 사용합니다.
캐시는 2차원 배열로 선언하여 두 개의 인덱스로 sub array가 시작하는 지점과 끝나는 지점의 maxCoins 값을 저장하도록 합니다.

예를 들어, 캐시를 2차원 배열 `cache`에 저장한다면, `[1, 1, 2, 3, 4, 5, 6, 7, 1]`에서 `cache[2][4]`는 `[2, 3, 4]`의 maxCoins 값이, `cache[5][8]`는 `[5, 6, 7, 1]`의 maxCoins 값이 됩니다.

지금까지 알아본 것을 바탕으로 **memoization** <sub>top down</sub> 과 **tabulation** <sub>bottom up</sub> 방식의 솔루션을 작성해보겠습니다.



## Memoization
```js
var maxCoins = function(nums) {
  nums = [1, ...nums, 1];
  let len = nums.length;
  let cache = Array(len);
  for(let i = 0; i < len; ++i) cache[i] = Array(len);

  function burst(left, right) {
    if(left + 1 === right) return 0;
    if(cache[left][right]) return cache[left][right];
    
    let ret = 0;
    for(let i = left + 1; i < right; ++i) {
      let tmp = burst(left, i) + nums[left] * nums[i] * nums[right] + burst(i, right);
      ret = Math.max(ret, tmp);
    }
    return cache[left][right] = ret;
  }

  return burst(0, len - 1);
};
```

top-down 방식을 적용한 해답 코드입니다.
전체 문제 `burst(0, len - 1)`에서 재귀적으로 부분의 문제를 파고들어 가장 작은 단위의 문제에 도달하면 그 답을 바탕으로 자신을 호출한 윗단계의 문제를 해결합니다.

```js
let tmp = burst(left, i) + nums[left] * nums[i] * nums[right] + burst(i, right);
```

`burst()` 함수의 `for`문 내부에 있는 이 코드 덕분에 `i`를 기준으로 나눈 두 부분의 maxCoins 값을 나누어 계산할 수 있습니다.

또한 `nums[i]`가 맨 마지막에 빠지기 때문에 `nums[i]`는 무조건 `nums[left]`, `nums[right]`와 곱합니다.



## Tabulation
```js
var maxCoins = function(nums) {
  nums = [1, ...nums, 1];
  let len = nums.length;
  let cache = Array(len);
  for(let i = 0; i < len; ++i) cache[i] = Array(len).fill(0);

  for(let j = 2; j < len; ++j) {
    for(let left = 0; left < len - j; ++left) {
      let right = left + j;
      for(let i = left + 1; i < right; ++i) {
        let tmp = cache[left][i] + nums[left] * nums[i] * nums[right] + cache[i][right];
        cache[left][right] = Math.max(cache[left][right], tmp);
      }
    }
  }
  return cache[0][len - 1];
};
```

bottom-up 방식의 코드입니다.
sub arrray의 길이가 `3`일 때, `4`일 때, ... , `len - 1`일 때의 각 구간별 maxCoins 값을 계산하여 캐시에 저장합니다.

### 시간복잡도
**O(N<sup>3</sup>)**

세 반복문이 겹쳐있으므로 시간복잡도는 O(N<sup>3</sup>)이 됩니다.