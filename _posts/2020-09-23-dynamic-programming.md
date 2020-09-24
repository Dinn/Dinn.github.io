---
title: 'Algorithm \| Jump Game, Unique Paths, Coin Change'
date: 2020-09-23
last_modified_at: 2020-09-23T20:04:36

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

> 본 글은 [leetCode](https://leetcode.com/explore/interview/card/top-interview-questions-medium/111/dynamic-programming/)의 Jump Game / Unique Paths / Coin Change 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Jump Game
[문제 자세히 보기](https://leetcode.com/problems/jump-game/)

```js
var canJump = function(nums) {
  let end = nums.length - 1;
  for(let i = nums.length - 1; i >= 0; --i) {
    if(i + nums[i] >= end) end = i;
  }
  return end === 0;
};
```



## Unique Paths
[문제 자세히 보기](https://leetcode.com/explore/interview/card/top-interview-questions-medium/111/dynamic-programming/808/)


### Approach 1
`board[i][j] = board[i - 1][j] board[i][j - 1]`의 점화식을 기반으로 `board[m - 1][n - 1]`를 반복문을 통해 구하는 방법입니다. 

```js
var uniquePaths = function(m, n) {
  let board = Array(m);
  board[0] = Array(n).fill(1);
  for(let i = 1; i < m; ++i) board[i] = [1];

  for(let i = 1; i < m; ++i) {
    for(let j = 1; j < n; ++j) {
      board[i][j] = board[i - 1][j] + board[i][j - 1];
    }
  }

  return board[m - 1][n - 1];
};
```


### Approach 2
`uniquePaths(m, n)`의 값이 <sub>m + n - 2</sub>C<sub>n - 1</sub>임을 이용한 방법입니다.

```js
var uniquePaths = function(m, n) {
  let M = m + n - 2;
  let N = Math.min(n - 1, M - n + 1);
  let denominator = 1;
  let numerator = 1;
  for(let i = 0; i < N; ++i) numerator *= M--;
  while(N) denominator *= N--;
  return numerator / denominator;
};
```



## Coin Change
[문제 자세히 보기](https://leetcode.com/problems/coin-change/)

```js
var coinChange = function(coins, amount) {
  let dp = Array(amount + 1).fill(amount + 1);
  dp[0] = 0;
  for(let i = 1; i <= amount; ++i) {
    for(let coin of coins) {
      if(i - coin >= 0) 
        dp[i] = Math.min(dp[i], dp[i - coin] + 1);
    }
  }
  return dp[amount] > amount ? -1 : dp[amount];
};
```