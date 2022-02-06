---
title: 'JavaScript \| reduce와 spread operator를 함께 사용하면 안되는 이유'
excerpt: 
date: 2022-02-02
last_modified_at: 2022-02-06T22:28:20

category:
 - JavaScript

tags:
 - JavaScript
 - ECMAScript
 - JS
 - Array
---

얼마 전, `Array.prototype.reduce`와 **spread operator**를 함께 사용하는 코드에 대해 지적하는 글을 보았습니다.

저에게 reduce 함수는 유독 각별한 느낌을 주는데요, reduce를 사용할 때 작성한 코드가 클린 코드인지 확신이 안 설 때가 종종 있어 고민하느라 시간을 보냈던 기억이 있습니다.

그런 깊은 고민의 결론으로 reduce + spread 조합을 사용한 적도 있기에 그 글의 내용이 너무나 궁금했고, 차마 그냥 지나칠 수가 없었습니다.

우선 결론부터 얘기하자면, 
* **reduce와 spread를 함께 사용하는 것은 성능적으로 비효율적입니다.**
* reduce를 사용하여 새로운 테이블을 생성하고자 할 때는 **accumulator를 mutatable하게 사용하는 것이 더 성능적으로 뛰어납니다.**



## References
들어가기에 앞서, 출처는 아래와 같습니다.

> [The reduce ({...spread}) anti-pattern - RichSnapp.com](https://www.richsnapp.com/article/2019/06-09-reduce-spread-anti-pattern)
> 
> [Why using object spread with reduce probably a bad idea](https://prateeksurana.me/blog/why-using-object-spread-with-reduce-bad-idea/)

위 글들은 실제 자료와 함께 `reduce...spread` 패턴을 왜 쓰면 안되는지에 대해 자세하게 분석하고 있습니다.
저는 거기에 숟가락만 얹어 위 글의 내용을 간단하게 소개 및 정리하고자 하며 자세한 내용이나 근거 등은 출처를 직접 확인해주시면 감사하겠습니다.

더불어, 이전에 `Array.prototype.reduce`와 spread operator 각각에 대해 정리한 글이 있는데 이 둘을 잘 모르시다면 먼저 읽어보시길 추천드립니다.

> [JS에서 점점점(…)은 무엇일까?](https://dinn.github.io/javascript/js-dotdotdot/)
>
> [Array Methods 3](https://dinn.github.io/javascript/array-03/#reduce)



## reduce...spread
먼저 reduce와 spread 조합은 정확히 어떤 경우를 일컫는지부터 시작하겠습니다.

```ts
const items: Array<{ name: string, value: number }> = [
  { name: "Cindy", value: 0 },
  { name: "Dinn", value: 1 },
  // ...
]
```

이런 배열이 있다고 합시다.

이해를 돕기 위해 typescript 방식으로 타입을 정의했는데, 쉽게 `items`는 *name, value란 property를 지닌 객체의 배열* 이라고 보시면 되겠습니다.

reduce를 이용해서 배열 items를 통해 `name: value` 테이블을 얻어내고자 합니다.

```js
items.reduce((table, item) => ({...table, [item.name]: item.value}), {})
```

이런 식으로 reduce의 콜백 함수 내부에서 spread operator를 사용하는 패턴을 `reduce...spread`라 하겠습니다.

`reduce...spread`의 장점은 다음과 같습니다.
1. 코드가 간결하다.
1. 코드의 immutability를 유지할 수 있다.

JavaScript의 주된 사용처인 React 환경에서 Immutability는 중요한 원칙입니다.
React는 reference 비교를 통해 state update를 감지하기 때문에 setState는 immutable update 해줘야 하다는 것은 React 개발자라면 누구나 숙지해야 하는 사실일 겁니다.[^1]

React의 대표적인 상태관리 라이브러리인 Redux 또한 같은 이유로 Immutability를 매우 중요시하며, 상태 변경 시 pure function을 사용할 것을 강조합니다.[^2]

[^1]: 정확히는 `Object.is`를 비교 알고리즘으로 사용하긴 합니다. [React 공식 문서](https://reactjs.org/docs/hooks-reference.html#bailing-out-of-a-state-update)
[^2]: [Redux 공식 문서](https://redux.js.org/understanding/thinking-in-redux/three-principles#changes-are-made-with-pure-functions)



## 문제점 및 개선 방안
하지만 `reduce...spread`에는 치명적인 단점이 있습니다.
바로 v8 엔진 기준에서 성능적으로 **느린 속도**를 보여준다는 점입니다.

이유는 다음과 같습니다.
spread operator는 table 객체의 property를 iterate하기 때문에 사실상 이중 반복문을 사용하는 것과 같은 효율을 보여줍니다.
reduce가 외부 반복문, spread operator가 내부 반복문인 셈이죠.

그렇기에 결과적으로 `reduce...spread`는 O(N<sup>2</sup>)의 시간복잡도를 갖습니다.

만약 for문을 통해 `name: value` 테이블을 구하고자 한다면 다음과 같이 코드를 작성할 수 있겠습니다.

```js
const table = {}
for(const item of items) {
  table[item.name] = item.value
}
```

이 코드의 시간복잡도는 의심의 여지 없이 O(N)입니다.

`reduce...spread`에 비해 더 나은 성능을 보여주지만, mutable 하다는 점이 발목을 잡습니다.

for문은 side effect에 취약하기 때문에 코딩 스타일로써 사용을 제한하는 경우가 자주 있어 대중적인 사용은 어려워 보입니다.
당장 저희 팀도 [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript#iterators-and-generators)를 따르며 for문 대신 `Array.prototype.forEach`, `Array.prototype.map` 등과 같은 iteration method를 사용하고 있습니다.
또한 React 사용 시 반복문이 jsx문 내부에 inline으로 들어가는 것이 필요한 경우가 있어 모든 반복문을 for문으로만 구성하는 것에 한계가 있기도 합니다.

결국 `reduce...spread`의 대안으로는 reduce와 mutable update를 혼합한 절충안(이하 `reduce mutate`) 정도가 적절하다고 생각합니다.

```js
items.reduce((table, item) => {
  table[item.name] = item.value
  return table
}, {})
```

spread operator가 도입되기 이전엔 모두가 이렇게 사용했을 것입니다.
아마 spread operator가 등장한 이후에 pure function을 의식하여 `reduce...spread` 방식이 사용되지 않았나 싶습니다.
<sub>적어도 전 그런 의도로 사용했습니다...</sub>

알다시피 순수 함수(pure function)의 정의는 아래 두 가지 조건을 만족하는 함수를 의미합니다.
1. 동일한 arguments에 대해 항상 동일한 값을 리턴한다.
1. side effect가 없다.

`reduce...spread`의 콜백 함수는 위 두 조건을 만족하기 때문에 pure function 입니다.
하지만 `reduce mutation`의 콜백 함수는 accumulator를 mutable update하고 있고, 이로 인해 side effect가 발생하므로 pure function이 아닙니다.

그래도 mutable update의 대상인 `table`은 여전히 reduce 함수에서 초기값으로 설정한 `{}` 이기에 reduce 외부의 어떤 reference에도 영향을 미치지 않고 있습니다. 
reduce 수준에선 side effect가 발생하고 있지 않는 것이죠.

따지고보니 원래부터 사용하던 방식을 대안이라고 말하는 것도 조금 우습지만, `reduce mutate`는 `reduce...spread` 대신하여 성능과 immutability 모두를 확보할 수 있는 좋은 대안이 아닐까 싶습니다.