---
title: 'Algorithm \| Find Peak Element, Search in Rotated Sorted Array'
date: 2020-09-24
last_modified_at: 2020-09-24T19:23:59

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

> 본 글은 [leetCode](https://leetcode.com/explore/interview/card/top-interview-questions-medium/110/sorting-and-searching/)의 Find Peak Element / Search in Rotated Sorted Array 문제 솔루션을 JavaScript 코드로 정리한 글입니다.

## Find Peak Element
[문제 자세히 보기](https://leetcode.com/problems/find-peak-element/)

### Approach 1: Linear Scan
```js
var findPeakElement = function(nums) {
  for(let i = 0; i < nums.length - 1; ++i) {
    if(nums[i] > nums[i + 1]) return i;
  }
  return nums.length - 1;
};
```

### Approach 3: Iterative Binary Search
```js
var findPeakElement = function(nums) {
  let lo = 0;
  let hi = nums.length - 1;
  let mid;
  while(lo < hi) {
    mid = Math.floor((lo + hi) / 2);
    if(nums[mid] < nums[mid + 1]) lo = mid + 1;
    else hi = mid;
  }
  return lo;
};
```



## Search in Rotated Sorted Array
[문제 자세히 보기](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```js
var search = function(nums, target) {
  let lo = 0;
  let hi = nums.length - 1;
  while(lo <= hi) {
    let mid = Math.floor((lo + hi) / 2);
    if(nums[mid] === target) return mid;
    if(nums[mid] > target) {
      if(target < nums[lo] && nums[mid] >= nums[lo]) lo = mid + 1;
      else hi = mid - 1;
    } else  {
      if(target > nums[hi] && nums[mid] <= nums[hi]) hi = mid - 1;
      else lo = mid + 1;
    }
  }
  return -1;
};
```