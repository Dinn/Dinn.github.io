---
title: 'JavaScript \| JS에서 점점점(...)은 무엇일까?'
excerpt: 'Rest Paramenter와 Spread Operator에 대해 알아보겠습니다.'
date: 2020-04-04
last_modified_at: 2020-04-06T13:36:25

category:
 - JavaScript

tags:
 - JavaScript
 - ECMAScript
 - ES6
 - three dots in js
 - spread operator
 - rest operator
---

JavaScript에서 `...`를 처음 봤을 때 조금 난감했습니다. 

여러 개의 값을 일일이 써넣지 않아도 잘 들어가게 해주는 뭐 그런 것 같았는데 세부 syntax가 몇 가지 있는 것 같더라구요.

구글이 `...`를 검색어로 인식을 못해서 처음엔 어떻게 검색하는지 감조차 안 잡혔습니다.

영어로는 'three dots'라고 검색하는 것 같습니다. 그렇게 검색해보면 요 `...`의 이름은 **rest parameter**와 **spread operator**라고 나옵니다.

그러니깐, `...`은 rest parameter나 spread operator로써 두 가지 상황에서 쓰일 수 있는 것 입니다.

## Abstract
Rest Parameter
: argument들이 배열로 함수 내부에 전달된다.

Spread Parmeter
: 배열이 개별 원소가 된다.

```js
let array = [3, 4, 5]

let newArray = [...array];
array.push(...[6, 7]);              // [3, 4, 5, 6, 7]
array.unshift(...[1, 2]);           // [1, 2, 3, 4, 5, 6, 7]

let arr1 = [1, 2];
let arr2 = [6, 7];
let arr3 = [...arr1, 3, arr2];      // [1, 2, 3, 6, 7]
arr3.splice(3, 0, ...[4, 5]);       // [1, 2, 3, 4, 5, 6, 7]
```



## Rest Parameter
이름에서 알 수 있듯이 parameter로 사용합니다. 

함수를 선언할 때 rest parameter를 사용하면, function call 할 때 개수에 상관없이 argument를 넣을 수 있습니다. 그러면 함수는 그 argument들을 배열로 받아옵니다.

```js
function passRestParam(a, b, ...args) {
    console.log(a);
    console.log(b);
    console.log(args);
}
passRestParam(1, 2, 3, 4, 5);
// 1
// 2
// [3, 4, 5]

function passRestParam(a, ...args, b) {
    // syntaxError: Rest parameter must be last formal parameter
}
```

rest parameter는 **함수의 마지막 parameter**로 밖에 쓸 수 없습니다.

몇 개이고 상관없이 값을 받을 수 있어서 rest parameter 뒤에 다른 parameter를 받아야 할 땐 어디까지가 rest parameter이고 어디부터가 다음 parameter인지를 알 수 없을테니 당연한 규칙인 것 같습니다.


### rest parameter vs. arguments

JavaScript에는 함수에 들어온 모든 argument 값들을 갖는 `arguments`라는 객체가 있습니다. 함수의 내부에서 지역 변수처럼 사용할 수 있는 `arguments` 객체는 `length` 속성이 있고 argument들을 전달된 순서대로 0부터 n까지 키를 부여한 **유사 배열 객체(array-like object)** 입니다.

덕분에 `arguments` 객체는 iterable하게 객체 내부를 순회할 수 있지만 배열이 아니기 때문에 `map()`, `reduce()`, `pop()` 등과 같은 메소드를 쓸 수 없습니다.

반면에 **rest parameter는 배열**이기 때문에 `arguments` 객체를 배열로 변환할 필요없이 바로 argument의 배열에 JavaScript의 Array instance method를 적용할 수 있다는 점에서 유용하게 쓰일 수 있습니다.

```js
function sumWithRest(... args) {
    for(let i = 0 ; i < args.length; ++i) console.log(args[i]);
    return args.reduce((acc, cur) => acc + cur);
}
sumWithRest(1, 2, 3, 4);
// 1
// 2
// 3
// 4
// 10        

function sumWithArgs(a, b, c) {
    for(let i = 0; i < arguments.length; ++i) console.log(arguments[i]);
    return arguments.reduce((acc, cur) => acc + cur);
}
sumWithArgs(1, 2, 3, 4);
// 1
// 2
// 3
// 4
// Uncaught TypeError: arguments.reduce is not a function
```


### Destructuring rest parameters
Destructuring은 우리말로 '구조 분해'라고 번역되는데 썩 와닿지는 않습니다.

Destructuring이란, 파괴한다는 의미에 걸맞게 배열 또는 객체를 쪼개서 그 값들을 개별적인 변수에 할당해주는 기법을 의미합니다.

```js
let arr1 = [1, 2, 3];
let [one, two, three] = arr1;
console.log(one, two, three);           // 1 2 3
```
보이는 바와 같이 배열 `arr1`, `arr2`,의 원소들을 각각 변수들에 할당해주는 것으로 배열을 해체하는 것과 같은 결과를 얻었습니다.

rest parameter를 사용한다면 함수 안에서 argument들의 배열을 array destructuring 한 효과를 얻을 수 있습니다.

```js
function destructuringParam(... [a, b, c]) {
    console.log(a, b, c);
}
destructuringParam(1, 2, 3);         // 1 2 3
```

rest parameter를 `...args`의 형태가 아닌 `...[a, b, c]`의 형태로 사용하여 배열 안의 `a`, `b`, `c`가 그대로 destructuring되어 함수 안에서 사용됩니다.



## Spread Operator
`String`, `Array`, `TypedArray`, `Map`, `Set` 등과 같이 iterable protocol을 준수하는 객체를 **[iterable]**이라고 합니다. 간단히 `for..of` 구조에서 반복문을 돌릴 수 있는 객체들이라고 생각하면 편하겠습니다.

**spread operator**는 이런 iterable을 개별 요소로 분리하는 연산자 입니다.

```js
console.log(1, 2, 3);           // 1 2 3

let arr = [1, 2, 3];
console.log(arr);               // [1, 2, 3]
console.log(...arr);            // 1 2 3
```

ES5까지는 배열의 각 element들을 하나씩 함수의 argument로 집어넣기 위해선 `Function.prototype.apply`를 사용해야 했습니다.

하지만 ES6에 spread operator가 추가됨으로 인해서 그런 번거로운 과정이 간편해졌습니다.

게다가 `apply` 함수는 `new` 키워드를 사용해 생성자를 호출할 땐 사용할 수 없다는 단점이 있었지만, 이 또한 speard operator로 생성자에 바로 배열을 집어넣는 것이 가능해졌습니다.

```js
Math.max(1, 2, 3);                      // 3

let arr = [1, 2, 3];
Math.max(arr);                          // NaN
Math.max.apply(null, arr);              // Math.max(1, 2, 3) -> 3
Math.max(...arr);                       // Math.max(1, 2, 3) -> 3


let array1 = new Array(1, 2, 3);        // [1, 2, 3]
let array2 = new Array.apply(arr);      // Uncaught TypeError: Array.apply is not a constructor
let array3 = new Array(...arr);         // [1, 2, 3]
```


### 배열에서의 spread operator
spread operator는 배열과의 궁합이 참 좋은 것 같습니다. Array instance method 중에선 optional하게 여러 값을 넣을 수 있는 함수들이 많은데, spread operator로 배열의 값들을 바로 함수에 넣을 수 있기 때문입니다.

다음 메소드들에서 spread operator가 어떻게 사용되는지 다뤄보겠습니다.

- `Array.from()`
- `Array.prototype.push()`
- `Array.prototype.unshift()`
- `Array.prototype.concat()`
- `Array.prototype.splice()`


#### Array.from()
`Array.from()`은 유사 배열 객체(arrayLike) 등의 iterable을 얕게 복사하여 `Array`객체로 만들어 주는 메소드 입니다. 

spread operator를 사용하여 Array.from()을 대체해보겠습니다.

```js
let str = 'string';

let arr1 = Array.from(str);             // ["s", "t", "r", "i", "n", "g"]
let arr2 = [...str];                    // ["s", "t", "r", "i", "n", "g"]

// create an array filled with 0 to 99
Array.from(Array(100).keys());          // [0, 1, ..., 99]
[...Array(100).keys()];                 // [0, 1, ..., 99]
```


#### Array.prototype.push() / Array.prototype.unshift()
배열에 새로운 원소를 추가할 수 있는 `Array.prototype.push()` 와 `Array.prototype.unshift()`는 argument 개수의 제한 없이 들어오는 값들을 모두 배열에 추가해줍니다.

덕분에 spread operator를 이용하여 배열을 통째로 다른 배열 뒤에 push하거나 앞 쪽에 unshift 해줄 수 있습니다.

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = [7, 8, 9];

arr1.push(...arr2);         // [1, 2, 3, 4, 5, 6]
arr3.unshift(...arr1);      // [1, 2, 3, 4, 5, 6, 7, 8, 9];
```


#### Array.prototype.concat() / Array.prototype.splice()
배열을 뒤에 덧붙이는 `Array.prototype.concat()`은 spread operator로 대체할 수 있습니다.

또한 `Array.prototype.splice()`와 spread oeprator로 `apply`를 사용하지 않고도 배열 도중에 배열을 집어넣을 수 있습니다.

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

arr1.concat(arr2);                        // [1, 2, 3, 4, 5, 6]
let concatedArr = [...arr1, ...arr2];     // [1, 2, 3, 4, 5, 6]

let arr3 = ['a', 'b', 'c', 'f', 'g'];
let arr4 = [...arr3];
let arr5 = ['d', 'e'];

Array.prototype.splice.apply(arr3, [3, 0].concat(arr5));    // ['a', 'b', 'c', 'd', 'e', 'f', 'g']
arr4.splice(3, 0, ...arr5);                           // ['a', 'b', 'c', 'd', 'e', 'f', 'g']
```
`[...arr3]`를 하여 `arr3`를 복사 한 뒤(swallow copy) `arr3`엔 `apply`를 이용한 기존 방식의 `splice()`를, `arr4`에는 spread operator를 이용한 `splice()`를 적용했습니다.



## References
> [Extended Parameter Handling \| PoiemaWeb](https://poiemaweb.com/es6-extended-parameter-handling)
>
> [Destructuring \| PoiemaWeb](https://poiemaweb.com/es6-destructuring)
>
> [Rest parameters - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
>
> [Spread syntax - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
>
> [Array - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)


[iterable]: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Iteration_protocols