---
title: 'Algorithm \| Generate Parentheses'
date: 2020-09-25
last_modified_at: 2020-10-09T14:40:49

category:
  - Algorithm

tags:
  - Algorithm
  - JavaScript
  - JS
  - ECMAScript
  - ES6
  - leetCode
  - Top Interview Questions Medium Collenction
---

> 본 글은 leetCode의 [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.



## Approach 2: Backtracking
재귀 함수를 통해 가능한 경우의 수를 순회하며 답을 구합니다.

순회는 `n = 3`일 때 `((()))` -> `(()())` -> `(())()` -> `()(())` ... 와 같이 제일 마지막에 등장하는 열린 괄호를 닫힌 괄호로 바꾸는 순서로 진행합니다.


```js
var generateParenthesis = function(n) {
  let ret = [];
  (function backtrack(str, open, close) {
    if(str.length === 2 * n) {
      ret.push(str);
      return;
    }
    if(open < n) backtrack(str + "(", open + 1, close);
    if(close < open) backtrack(str + ")", open, close + 1);
  })("", 0, 0);

  return ret;
};
```



## Approach 3: Closure Number
```
parens(n) = [
  "(" + parens(0) + ")" + parens(n - 1),
  "(" + parens(1) + ")" + parens(n - 2),
  "(" + parens(2) + ")" + parens(n - 3),
    ... 
  "(" + parens(n - 1) + ")" + parens(0)
]
```

 인 성질을 이용해서 문제를 해결해보겠습니다.

```js
var generateParenthesis = function(n) {
  if(n == 0) return [""];
  let ret = [];
  for(let i = 0; i < n; ++i) {
    for(let left of generateParenthesis(i)) {
      for(let right of generateParenthesis(n - i - 1)) {
        ret.push(`(${left})${right}`);
      }
    }
  }
  return ret;
};
```

이 상태에서 캐시를 추가하여 dynamic programming을 적용해 볼 수도 있겠습니다.