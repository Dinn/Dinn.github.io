---
title: 'Algorithm \| Power of Three'
date: 2020-09-19
last_modified_at: 2020-09-19T18:06:41

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
  - Power of Three
---

> 본 글은 leetCode의 [Power of Three](https://leetcode.com/problems/power-of-three/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## 문제: Power of Three
[문제 자세히 보기](https://leetcode.com/problems/power-of-three/)



## Approach 1: Loop Iteration
가장 naive한 방법입니다.

문제의 의미를 살려서 n진수를 만들 듯이 3으로 나누고, 나머지를 확인합니다.

주어진 수가 3의 거듭제곱이기 위해선 입력값이 1이 되기 전까진 아무리 나눠도 나머지가 나오면 안됩니다.

```js
var isPowerOfThree = function(n) {
    while(n % 3 === 0 && n > 1) {
        n /= 3;
    }
    return n === 1
};
```

<br>
**시간복잡도** O(logN)



## Approach 2: Base Conversion
n의 거듭제곱을 n진수로 표현하면 1000<sub>n</sub>과 같이 가장 큰 자리수만 1이고 나머지는 모두 0인 숫자의 패턴입니다.

* 10<sup>0</sup> -> 1
* 10<sup>1</sup> -> 10
* 10<sup>2</sup> -> 100
* 10<sup>3</sup> -> 1000

* 2<sup>0</sup> -> 1 = 1<sub>2</sub>
* 2<sup>1</sup> -> 2 = 10<sub>2</sub>
* 2<sup>2</sup> -> 4 = 100<sub>2</sub>
* 2<sup>3</sup> -> 8 = 1000<sub>2</sub>

이러한 사실을 이용하여, 입력값을 3진수로 변환한 뒤에 문자열이 `1000...` 의 패턴을 따르는지 확인하여 답을 구해보겠습니다.

JS에서 `Number.prototype.toString(n)` 함수가 숫자를 입력받은 n진수의 문자열로 변환해줍니다.

또한 `String.prototype.match()` 함수를 이용하여 문자열의 일치여부를 판단하겠습니다.

```js
var isPowerOfThree = function(n) {
    return Boolean(n.toString(3).match(/^10*$/));
};
```



## Approach 3: Mathematics
로그 개념을 활용하는 방법입니다.

3의 거듭제곱인 수 n에 대한 log<sub>3</sub>(n)의 값은 정수라는 성질을 이용하는 것입니다.
log<sub>3</sub>(n)은 아래와 같이 밑변환 공식을 사용하여 계산합니다.

![logarithm](/assets/images/2020-09-19-power-of-three/logarithm-change-of-base.svg)

```js
var isPowerOfThree = function(n) {
    return Number.isInteger(Math.log10(n) / Math.log10(3));
};
```

> 원문에 따르면, (Java 기준으로) log 값을 부동소수점으로 처리하기 때문에 `epsilon`을 사용해 오차를 보정해야 합니다.
>
> 하지만 확인 결과 JS에서 표현할 수 있는 정수 범위 내에서 `Math.log10(n) / Math.log10(3)`을 했을 때 오차가 발생할 때는 3<sup>27</sup> = 7625597484987 밖에 없었습니다.
> 이는 4 byte 크기의 int형 정수가 표현할 수 있는 범위를 이미 넘은 수입니다.
>
> 또한, 계산해보니 `Math.log10(7625597484987) / Math.log10(3)`가  `17 * Number.EPSILON` 정도의 오차를 갖는데 `17 * Number.EPSILON` 수준의 오차 범위가 적절한지도 확실히 모르겠더라구요.
>
> 이래저래 생각해봤을 때 `Math.log10(n) / Math.log10(3)`를 그대로 써도 되지 않나 생각합니다.ㅎㅎ


## Approach 4: Integer Limitation
표현할 수 있는 가장 큰 3의 거듭제곱을 입력값 n으로 나누는 방법입니다.

좀 더 자세히 설명하자면,

우선 표현할 수 있는 범위 중에서 가장 큰 3의 거듭제곱을 구합니다.

4 byte의 int형 숫자를 사용하는 언어에선 수의 최대값은 2<sup>31</sup> - 1 일 것이며, JS에서는 2<sup>53</sup> - 1 (`Number.MAX_SAFE_INTEGER`) 입니다.[^1]

이 범위 안에서 가장 큰 3의 거듭제곱은 3<sup>33</sup> = 5559060566555523 입니다.

주어진 수가 3의 거듭제곱이라면 3<sup>33</sup>를 나눴을 때 나머지는 0이 됩니다.

```js
var isPowerOfThree = function(n) {
    return n > 0 && 5559060566555523 % n === 0
};
```

<br>
**시간복잡도** O(1)



## References
> [https://leetcode.com/problems/power-of-three/solution/](https://leetcode.com/problems/power-of-three/solution/)

[^1]: [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)