---
title: 'Algorithm \| Group Anagrams'
date: 2020-09-21
last_modified_at: 2020-09-21T19:48:58

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

> 본 글은 leetCode의 [Group Anagrams](https://leetcode.com/problems/group-anagrams/) 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Approach #1: Categorize by Sorted String
hash table에 단어를 정렬한 문자열을 key로, 단어의 배열을 value로 저장하는 방법입니다.

객체로 hash table을 대체했으며 `Object.values()` 함수를 이용하여 객체의 값들을 바로 2차원 배열의 형태로 리턴합니다.

```js
var groupAnagrams = function(strs) {
  let groups = {};
  for(let str of strs) {
    let key = str.split("").sort((a, b) => a.localeCompare(b)).join("");
    groups[key] ? groups[key].push(str) : groups[key] = [str];
  }
  return Object.values(groups);
};
```

### 시간복잡도
O(nk*log*k)

* **N** 배열 `strs`의 길이
* **K** 배열 `strs`의 원소 문자열의 길이 중 최대값

## Approach #2: Categorize by Count
이전 방법은 정렬한 문자열을 key로 사용했다면 이번엔 정렬을 하지 않고 key 값을 구합니다.

대신 anagram 끼리 같은 key 값을 갖도록 단어에 사용된 알파벳의 분포를 키로 사용합니다.

무슨 말이냐면, 길이가 26인 배열을 선언하고, 알파벳 순으로 `i` 번째 알파벳의 개수를 배열의 `i` 번째 원소에 저장하는 것입니다. 그리고 그 배열을 키로 사용하는 것이죠.

예를 들어, `"abc"` 라는 단어가 있다면 키 값은 `"#1#1#1#0#0#0..."` 이 되는 것입니다.

여기서, 숫자 사이에 `"#"`이 들어가는 이유는 알파벳의 사용 횟수만으로 문자열을 구성하면 같은 문자가 10회 이상 포함된 단어는 알파벳과 자리의 대응이 어려워지기 때문입니다.

가령, `"bbbbbbbbbbc"`와 `"bdddddddddd"`를 비교해보겠습니다.
각각 `b` 10번 `c` 1번, `b` 1번, `d` 10번 등장하는 문자열 입니다.

* `"bbbbbbbbbbc"` -> `"010100000000000000000000000"`
* `"bdddddddddd"` -> `"010100000000000000000000000"`

```js
var groupAnagrams = function(strs) {
  let groups = {};
  for(let str of strs) {
    let array = Array(26).fill(0);
    for(let ch of str) array[ch.charCodeAt() - "a".charCodeAt()]++;
    let key = array.join("#");
    groups[key] ? groups[key].push(str) : groups[key] = [str];
  }
  return Object.values(groups);
};
```

### 시간복잡도
O(NK)

* **N** 배열 `strs`의 길이
* **K** 배열 `strs`의 원소 문자열의 길이 중 최대값

## References
> [https://leetcode.com/problems/group-anagrams/solution/](https://leetcode.com/problems/group-anagrams/solution/)