---
title: 'Algorithm \| Word Break'
date: 2020-09-30
last_modified_at: 2020-09-30T15:32:23

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

> 본 글은 leetCode의 [Word Break](https://leetcode.com/problems/word-break/)와 [Word Break II](https://leetcode.com/problems/word-break-ii/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.


## Word Break
backtracking과 dynamic programming 기법을 이용하여 문제를 해결합니다.

backtracking으로 모든 경우의 수를 순회하며 결과값을 캐시에 저장하여 동일한 경우의 수는 중복하여 확인하지 않도록 하는 것입니다.

코드는 다음과 같습니다.

```js
var wordBreak = function(s, wordDict) {
  let cache = Array(s.length + 1).fill(null);
  
  function backtrack(i) {
    if(i === s.length) return true;
    if(cache[i] !== null) return cache[i];
    for(let word of wordDict) {
      if(!s.startsWith(word, i)) continue;
      if(backtrack(i + word.length)) return cache[i] = true;
    }
    return cache[i] = false;
  }

  return backtrack(0);
};
```



## Word Break II
이 문제는 위의 Word Break와는 다르게 답을 모두 배열에 저장해야 합니다.

그러기 위해 참거짓이었던 `backtrack()`의 리턴값을 배열로 바꿨습니다.
`backtrack()`이 리턴하는 배열의 각 원소에 일치하는 단어를 추가하기 위해서 입니다.

또한 이번에도 dynamic programming을 활용하여 문자열의 인덱스 별로 답을 기억하도록 했습니다.

```js
var wordBreak = function(s, wordDict) {
  let cache = [];

  function backtrack(i) {
    if(cache[i]) return cache[i];
    let ret = [];
    for(let word of wordDict) {
      if(!s.startsWith(word, i)) continue;
      if(i + word.length === s.length) {
        ret.push(word);
      } else {
        ret.push(...backtrack(i + word.length).map(e => word + " " + e));
      }
    }
    return cache[i] = ret;
  }

  return backtrack(0);
};
```