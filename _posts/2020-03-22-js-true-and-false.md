---
title: '참거짓 값과 Boolean객체'
excerpt:
date: 2020-03-28
last_modified_at:

category:
 - JavaScript

tags:
 - JavaScript
 - ECMAScript
 - JS
 - ES6
---


## Primitive Data Type
JavaScript에는 다음과 같이 총 7가지의 primitive data type이 있습니다.

`string`, `number`, `bigint`, `boolean`, `null`, `undefined`, `symbol`

이 중 `symbol`은 ECMAScript 2015에서 새로 추가된 자료형인데 주로 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티 키(property key)를 만들기 위해 사용한다고 합니다만[^1] 이에 대한 자세한 내용은 다음에 기회가 될 때 다루도록 하겠습니다.

**primitive data type(원시 자료형)**이란 객체나 메서드가 아닌 언어의 저레벨에서 직접 표현하고 다루는 데이터 입니다.

너무 추상적인 말이긴 하지만 저는 `number`는 그 자체가 `number`고 `string`은 그 자체가 `string`이라고 생각하고 넘어가는 식으로 이해하곤 합니다.

이런 primitive type의 데이터들은 wrapper 객체에 담아 처리할 수도 있습니다.

**wrapper객체**란 primitive data type들의 값을 지니고 그것과 관련된 메소드들을 처리할 수 있는 객체들로 primitive type들의 객체버전이라고 생각하면 되겠습니다. 이로 인해 언어 저레벨에서 정의되는 데이터들이 객체로도 취급될 수 있게 되면서 생성자(constructor)를 통한 선언이나 메소드(method) 사용이 가능해진다는 이점이 생깁니다.

서론이 길었는데 이번 글에서는 다른 언어와는 다르게 처리되는 **JavaScript의 참거짓**에 대해서 다뤄볼려고 합니다. true/false 값과 경우를 따지려다보니 `Boolean`객체에서의 경우 또한 빼놓을 수 없었으므로 primitive data type과 wrapper객체에 대해 미리 언급하였습니다.    


## True or False
### false
`0`, `-0`, `null`, `false`, `NaN`, `undefined`, `""`

### true
false값 이외의 모든 값


`0`, `-0`, `null`, `false`, `NaN`, `undefined`, `""`(빈 문자열) 의 진리값은 **false**입니다. 반대로 객체를 포함한, false 이외의 모든 값들은 **true**입니다.

이로 인해 몇 가지 혼동되는 경우가 존재합니다. 우선, 빈 문자열`""` 이외에 내용이 있는 문자열은 true이기 때문에 문자열 `'false'`는 true값을 가집니다. 

또한 argument의 진리값을 반환하는 함수 `Boolean()`는 false값이 들어왔을 때 그대로 false를 반환하지만 Boolean객체는 자신에게 담긴 값이 false이더라도 객체이므로 자체의 진리값은 true입니다.

```javascript
0                                       // false
-0                                      // false
1                                       // true
-1                                      // true

null                                    // false
NaN                                     // false
undefined                               // false
''                                      // false
'false'                                 // true
'undefined'                             // true

Boolean(false)                          // false(function)
var boolF = new Boolean(false)          // true(object)
boolF.valueof()                         // false
```


## References
> [Boolean \- JavaScript \| MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
>
> [원시값 \- 용어사전 \| MDN](https://developer.mozilla.org/ko/docs/Glossary/Primitive)
>
[^1]:https://poiemaweb.com/es6-symbol