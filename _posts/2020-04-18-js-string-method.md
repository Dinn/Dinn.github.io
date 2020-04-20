---
title: 'JavaScript \| String Methods'
excerpt: 'String intstances의 methods를 정리하였습니다.'
date: 2020-04-18
last_modified_at: 2020-04-20T19:10:00

category:
 - JavaScript

tags:
 - JavaScript
 - ECMAScript
 - JS
 - ES6
 - string instances methods
---

이번 글에선 String intstances의 methods를 다뤄보겠습니다.

참고로 길이를 구하는 `length`는 문자열 객체의 속성이기 때문에 함수를 호출하지 않고 그냥 `"string".length`의 형태로 사용하면 됩니다.

또한 HTML wrapper methods라 하여 HTML tag를 바로 만들어 주는 메소드가 있지만 현재의 웹 환경에서는 권장하지 않기 때문에 HTML wrapper methods는 다루지 않겠습니다.

중요한 점은, 문자열은 **변경 불가능(immutable)**한 원시 값(primitive value)이기 때문에 **String 객체의 모든 메소드는 언제나 새로운 문자열을 반환**한다는 것입니다.

본 글에서 다루는 String methods는 '펼치기'를 눌러 확인할 수 있습니다.

<details>
    <summary>펼치기</summary>
      <ul style="list-style-type: none">
        <li><code><a href="#stringprototypecharatindex">String.prototype.charAt()</a></code></li>
        <li><code><a href="#stringprototypeindexofsearchvalue--fromindex">String.prototype.indexOf()</a></code></li>
        <li><code><a href="#stringprototypelastindexofsearchvalue--fromindex">String.prototype.lastIndexOf()</a></code></li>
        <li><code><a href="#stringprototypecharcodeatindex">String.prototype.charCodeAt()</a></code></li>
        <li><code><a href="#stringprototypecodepointatpos">String.prototype.codePointAt()</a></code></li>
        <li><code><a href="#stringprototypenormalizeform">String.prototype.normalize()</a></code></li>
        <li><code><a href="#stringprototypeincludessearchstring--position">String.prototype.includes()</a></code></li>
        <li><code><a href="#stringprototypestartswithsearchstring--position">String.prototype.startsWith()</a></code></li>
        <li><code><a href="#stringprototypeendswithsearchstring--position">String.prototype.endsWith()</a></code></li>
        <li><code><a href="#stringprototypelocalecomparecomparestring--locales--options">String.prototype.localeCompare()</a></code></li>
        <li><code><a href="#stringprototypeconcatstr--strn">String.prototype.concat()</a></code></li>
        <li><code><a href="#stringprototypesubstringbegin--end">String.prototype.substring()</a></code></li>
        <li><code><a href="#stringprototypeslicebegin--end">String.prototype.slice()</a></code></li>
        <li><code><a href="#stringprototyperepeatcount">String.prototype.repeat()</a></code></li>
        <li><code><a href="#stringprototypepadstarttargetlength--padstring">String.prototype.padStart()</a></code></li>
        <li><code><a href="#stringprototypepadendtargetlength--padstring">String.prototype.padEnd()</a></code></li>
        <li><code><a href="#stringprototypetrim">String.prototype.trim()</a></code></li>
        <li><code><a href="#stringprototypesearchregexp">String.prototype.search()</a></code></li>
        <li><code><a href="#stringprototypematchregexp">String.prototype.match()</a></code></li>
        <li><code><a href="#stringprototypematchallregexp">String.prototype.matchAll()</a></code></li>
        <li><code><a href="#stringprototypereplacesearchfor-replacewith">String.prototype.replace()</a></code></li>
        <li><code><a href="#stringprototypesplitseperator--limit">String.prototype.split()</a></code></li>
        <li><code><a href="#stringprototypetostring">String.prototype.toString()</a></code></li>
        <li><code><a href="#stringprototypetolowercase">String.prototype.toLowerCase()</a></code></li>
        <li><code><a href="#stringprototypetouppercase">String.prototype.toUpperCase()</a></code></li>
        <li><code><a href="#stringportotypevalueof">String.prototype.valueOf()</a></code></li>
      </ul>
</details>

## charAt / indexOf / lastIndexOf

#### `String.prototype.charAt(index)`
`index`에 위치한 문자를 반환합니다. `index`가 문자열의 길이를 벗어났을 경우엔 빈 문자열을 반환합니다.

#### `String.prototype.indexOf(searchValue[, fromIndex])`
문자열에서 `searchValue`와 일치하는 첫번째 문자의 위치를 반환하며 일치하는 문자가 없을 시 `-1`을 반환합니다. `fromIndex`를 사용하여 검색을 시작할 위치를 지정해줄 수 있습니다.

#### `String.prototype.lastIndexOf(searchValue[, fromIndex])`
문자열에서 `searchValue`와 일치하는 마지막 문자의 위치를 반환하며 일치하는 문자가 없을 시 `-1`을 반환합니다. `fromIndex`를 사용하여 검색을 시작할 위치를 지정해줄 수 있습니다.



## charCodeAt / codePointAt / normalize

#### `String.prototype.charCodeAt(index)`
`index`에 위치한 문자의 UTF-16 코드를 반환합니다. 이를 통해 ASCII코드를 얻을 수도 있습니다. `index`가 문자의 길이를 벗어난다면 `NaN`을 반환합니다.

#### `String.prototype.codePointAt(pos)`
`pos`에 위치한 문자의 UTF-16 코드를 반환하며 `pos`에 어떤 값도 없을 시엔 `undefined`를 반환합니다.

#### `String.prototype.normalize([form])`
주어진 문자열에 Unicode Normalization 처리를 한 문자열을 반환합니다.



## includes / startsWith / endsWith

#### `String.prototype.includes(searchString[, position])`
문자열에 `searchString`이 있는지 여부를 판단합니다. 있다면 `true`를, 없다면 `false`를 반환하며 `position`으로 검색을 시작하는 위치를 설정할 수 있습니다.

#### `String.prototype.startsWith(searchString[, position])`
문자열이 `searchString`으로 시작하는지 여부를 판단합니다. `searchString`으로 시작한다면 `true`를, 아니면 `false`를 반환하며 `position`으로 시작 위치를 설정할 수 있습니다.

#### `String.prototype.endsWith(searchString[, position])`
문자열이 `searchString`으로 끝나는지 여부를 판단합니다. `searchString`으로 끝난다면 `true`를, 아니면 `false`를 반환하며 `position`으로 끝 위치를 설정할 수 있습니다.

```js
let str = 'Rolling stone gathers no moss';
str.includes('no');           // true;
str.includes('yes');          // false;

str.startsWith('Roll');       // true;
str.startsWith('stone');      // false;
str.startsWith('stone', 8);   // true;

str.endsWith('moss');         // true;
str.endsWith('moss', 3);      // false;
```



## localeCompare

#### `String.prototype.localeCompare(compareString[, locales[, options]])`
두 문자열을 비교해서 같다면 `0`, 사전 순으로 `compareString`이 먼저 나오면 양수, 뒤에 나오면 음수를 반환합니다.

```js
let str1 = 'aaa'
let str2 = 'bbb'
let str3 = 'bbc'

str2.localeCompare(str1);     // 1  (str2 - str1)
str2.localeCompare(str2);     // 0  (str2 - str2)
str2.localeCompare(str3);     // -1 (str2 - str3)
```



## concat / substring / slice

#### `String.prototype.concat(str[, ...strN])`
문자열 뒤에 입력 받은 문자열을 붙여 새로운 문자열을 반환합니다.

#### `String.prototype.substring(begin[, end])`
`begin`와 `end` 사이에 있는 문자열을 반환합니다. `substr()`의 사용은 권장되지 않습니다.

#### `String.prototype.slice(begin[, end])`
`begin`와 `end` 사이에 있는 문자열을 반환합니다.


### `substring` vs. `slice`

#### Commons
* 추출하는 문자열의 범위는 `begin` 이상 `end` 미만입니다. [`begin`, `end`)
* `begin`와 `end`의 기본값은 각각 문자열의 시작과 끝입니다.
* `NaN`을 입력받으면 `0`으로 처리합니다.

#### Differences
1. `substring`
  * `begin === end` 이면 빈 문자열을 반환하고, `begin > end` 이면 두 값을 바꿔서 `substring()`을 합니다.
  * 입력한 인덱스 값이 `0`보다 작으면 `0`으로, 문자열의 길이보다 크면 마지막 인덱스로 값을 처리합니다.

1. `slice`
  * `being >= end` 이면 빈 문자열을 반환합니다.
  * 입력한 인덱스 값이 0보다 작으면 문자열의 뒤에서 입력한 인덱스 번째가 그 위치가 되며, 문자열의 길이보다 크면 마지막 인덱스로 값을 처리합니다.

```js
let str1 = 'abc';
let str2 = 'def';
str1.concat(str2);        // 'abcdef'

let str = '0123456789';
str.substring();          // '0123456789'
str.slice();              // '0123456789'

str.substring(2, 7);      // '23456'
str.slice(NaN, 5);        // '01234'  str.slice(0, 5);

str.substring(7, 2);      // '23456'  str.substring(2, 7);
str.substring(3, 3);      // ''
str.substring(-5, 15);    // '0123456789'  str.substring(0, str.length - 1);

str.slice(7, 2);          // ''
str.slice(3, 3);          // ''
str.slice(-5, 15);        // '56789'  str.slice(str.length - 5, str.length - 1);
```



## repeat / padStart / padEnd

#### `String.prototype.repeat(count)`
문자열이 `count`번 만큼 반복되는 문자열을 반환합니다.

#### `String.prototype.padStart(targetLength[, padString])`
현재의 문자열 앞에 `padString`이 반복적으로 오는, 길이가 `targetLenth`인 문자열을 반환합니다. 

#### `String.prototype.padEnd(targetLength[, padString])`
현재의 문자열 뒤에 `padString`이 반복적으로 오는, 길이가 `targetLenth`인 문자열을 반환합니다. 

```js
let chorus = "Because I'm happy. "
chorus.repeat(5);
// "Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. Because I'm happy. "

let name = 'PARK JUNGHYUK';
let last4Chars = name.slice(-4);
last4Chars.padStart(name.length, '*');    // "*********HYUK"

let first4Chars = name.slice(0, 4);
first4Chars.padEnd(name.length, '*');     // "PARK*********"
```



## trim

#### `String.prototype.trim()`
앞뒤에 있는 공백문자들을 제거한 문자열을 반환합니다.

#### `String.prototype.trimStart()` / `String.prototype.trimLeft()`
앞쪽(왼쪽)에 있는 공백문자들을 제거한 문자열을 반환합니다.

#### `String.prototype.trimEnd()` / `String.prototype.trimRight()`
뒷쪽(왼쪽)에 있는 공백문자들을 제거한 문자열을 반환합니다.



## search / match / matchAll / replace

#### `String.prototype.search(regexp)`
문자열과 `regexp`이 첫번째로 일치하는 부분의 맨 앞 위치를 반환합니다. 일치하는 부분이 없을 땐 `-1`을 반환합니다.

#### `String.prototype.match(regexp)`
`regexp`와 일차하는 문자열을 반환합니다. 단, `regexp`에 `/g` flag(global flag)를 붙였을 경우, 일치하는 모든 문자열을 배열로 반환하며 `/g` flag를 붙이지 않았을 경우엔 일치하는 맨 첫 문자열만을 반환합니다. 일치하는 결과가 없을 때엔 `null`을 반환합니다.

#### `String.prototype.matchAll(regexp)`
`regexp`와 일치하는 문자열을 모두 반환합니다. 하지만 `regexp`에 `/g` flag가 없을 경우 `TypeError`가 발생합니다.

`search()`, `match()`, `matchAll()`의 parameter는 정규표현식(regular expression)이며, 문자열 같이 정규표현식이 아닌 객체가 들어오게 되면 정규표현식으로 형변환 합니다.

#### `String.prototype.replace(searchFor, replaceWith)`
문자열에서 `searchFor`와 일치하는 부분을 `replaceWith`으로 바꿔줍니다. 만약 `searchFor`가 정규표현식이라면 일치하는 모든 결과를 바꿔주지만 문자열이라면 일치하는 맨 앞의 결과 만을 바꿔줍니다. 또한 `replaceWith`에는 문자열이나 함수가 올 수 있습니다.

```js
let str = 'ABCDEFG abcdefg';
str.search(/b..e/gi);           // 1
str.match(/b..e/gi);            // ["BCDE", "bcde"]
str.replace(/b..e/gi, '   ');   // "A    FG a    fg"
```



## split / toString

#### `String.prototype.split([seperator[, limit])`
`seperator`를 기준으로 쪼갠 문자열로 구성된 배열을 반환합니다. `seperator`는 문자열이나 정규표현식이 올 수 있으며 `seperator`가 빈 문자열이라면 한 글자 씩으로 쪼개진 배열을 얻을 수 있습니다. `limit`를 입력한다면 배열의 크기가 `limit`일 때까지만 `split()`을 수행합니다.

#### `String.prototype.toString()`
객체를 텍스트 값으로 표시하는 문자열을 반환합니다.

```js
let str = "How are you doing?";
str.split(' ');       // ["How", "are", "you", "doing?"]
str.split(/\s/);      // ["How", "are", "you", "doing?"]
str.split();          // ["How are you doing?"]
str.split('', 8);     // ["H", "o", "w", " ", "a", "r", "e", " "]

Number(1234).toString();    // "1234"
```



## toLowerCase / toUpperCase

#### `String.prototype.toLowerCase()`
모든 문자를 소문자로 변환한 문자열을 반환합니다.

#### `String.prototype.toUpperCase()`
모든 문자를 대문자로 변환한 문자열을 반환합니다.

#### `String.prototype.toLocaleLowerCase([...locale])`
지정한 `locale`에서 모든 문자를 소문자로 변환한 문자열을 반환합니다.

#### `String.prototype.toLocaeUpperCase([...locale])`
지정한 `locale`에서 모든 문자를 대문자로 변환한 문자열을 반환합니다.



## valueOf

#### `string.portotype.valueOf()`
문자열 객체의 primitive value를 반환합니다.



## References
> [String - JavaScript \| MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
>
> [String \| PoiemaWeb](https://poiemaweb.com/js-string)


