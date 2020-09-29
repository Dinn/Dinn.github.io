---
title: 'Algorithm \| Best Time to Buy and Sell Stock with Cooldown'
date: 2020-09-29
last_modified_at: 2020-09-29T19:51:20

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

> 본 글은 leetCode의 [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

```js
var maxProfit = function(prices) {
  let buy = -prices[0];
  let rest = 0;
  let sell = 0;
  for(let p of prices) {
    buy = Math.max(buy, rest - p);
    rest = Math.max(rest, sell);
    sell = Math.max(sell, buy + p);
  }
  return sell;
};
```

`buy`, `rest`, `sell` 값의 단위는 모두 주식 거래의 수익입니다. 하지만 세부적인 의미는 조금 다릅니다.

#### `buy`는 주식을 산 시점의 수익입니다.
주식을 팔고서 최소 하루를 쉬어야지 다시 살 수 있지만 어느 시점에 사는 것이 수익을 최대화하는지 모르기 때문에 사는 시점을 기억해뒀다가 비교하는 과정이 필요합니다.
이때 이전 `buy`와 `rest - p` 값을 비교하여 더 높은 값이 매수가가 됩니다.
`rest - p`는 거래를 쉬는 동안의 수익인 `rest`에서 현재 가격 `p`를 빼서 오늘 주식을 매수했을 때의 수익입니다.
이를 통해 도중에 쉬는 경우와 쉬지 않는 경우 중 더 많은 수익이 나는 쪽을 찾을 수 있습니다.

#### `rest`는 거래를 쉬는 시점의 수익입니다.
`rest`는 for loop을 반복할 때마다 `rest`와 `sell` 중 큰 값으로 갱신됩니다.
`sell`은 주가가 극대값에 이를 때까지 계속 상승하기 때문에 `rest`도 함께 계속 증가합니다.
그러다가 `p`가 이전보다 낮아지면 구간 안에서 수익의 최대값을 `sell`과 `rest`에 저장합니다.
이후 거래를 계속 해가며 `sell` 값은 변화하고 거래를 쉬는 시점의 수익인 `rest`와 새로운 거래로 인한 수익인 `sell`을 비교하여 특정 시점에서 쉬는 것이 더 이득인지 판단합니다.


#### `sell`은 주식을 판 시점의 수익입니다.
`sell = buy + p`입니다.
주식을 산 시점의 수익인 `buy`와 현재 주식 가격 `p`를 더하여 판 시점의 수익을 구합니다.
단, 현재의 `sell`과 `buy + p`를 비교해서 현재의 `sell > buy + p`인 경우엔 `sell`을 대체하지 않습니다.
더 낮은 수익이 발생하면 거래를 하지 않는 것입니다. 

`buy`가 고정된 상태에서 `p`가 증가할수록 `sell` 또한 계속해서 증가합니다.
그리고 `p` 값이 전날 가격보다 떨어지는 순간 `sell < buy + p`이 성립합니다.
이런 식으로 수익의 최대값을 `sell`에 남기는 것이 가능해집니다.