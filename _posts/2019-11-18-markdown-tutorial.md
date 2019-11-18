---
title: "마크다운 사용법 / Markdown Tutorial"
excerpt: "마크다운 기본 사용법"

categories:
  - Dev Tips
tags:
  - Markdown
  - tutorial
  - cheatsheet
  - 마크다운
---
<style>
pre {
    background-color: whitesmoke
}

pre.highlight {
    background-color: #263238: 
}
</style>

## Headers
<pre>
# H1
## H2
### H3
#### H4
##### H5
###### H6

Alt-H1
======

Alt-H2
------
</pre>

# H1
## H2
### H3
#### H4
##### H5
###### H6

Alt-H1
======

Alt-H2
------


## Emphasis
<pre>
italic: 
*asterisks* or _underscores_

bold: 
**asterisks** or __underscores__

scratch: 
~~tildes~~
</pre>

italic: 
*asterisks* or _underscores_

bold: 
**asterisks** or __underscores__

scratch: 
~~tildes~~


## Lists
### Unordered list
<pre>
* asterisk
+ plus
- minus
  * unordered sub-list
    + unordered sub-sub-list
    - unordered sub-sub-list
</pre>
* asterisks
+ plus
- minus
  * unordered sub-list
    + unordered sub-sub-list
    - unordered sub-sub-list
    
### ordered list
<pre>
1. A
1. B
    * mixed list1
    * mixed list2
1. C
    1. C1
    1. C2
</pre>
1. A
1. B
    * mixed list1
    * mixed list2
1. C
    1. C1
    1. C2


## Links
<pre>
[inline link](https://dinn.github.io)

[inline link with title](https://dinn.github.com "저녁밥 블로그")

[relative reference to a repository file](../docs/_docs/01-quick-start-guide.md)

[referenced link][reference text]

[number reference][1]

[text itself]

[reference text]: https://google.com "Google"
[1]: https://developer.mozilla.org
[text itself]: https://github.com
</pre>
[inline link](https://dinn.github.io)

[inline link with title](https://dinn.github.com "저녁밥 블로그")

[relative reference to a repository file](../docs/_docs/01-quick-start-guide.md)

[referenced link][reference text]

[number reference][1]

[text itself]

[reference text]: https://google.com "Google"
[1]: https://developer.mozilla.org
[text itself]: https://github.com


## Images
<pre>
Inline-style: 
![alt text](https://www.w3schools.com/w3css/img_lights.jpg "aurora")

Reference-style:
![alt text][img]

[img]: https://helpx.adobe.com/content/dam/help/en/stock/how-to/visual-reverse-image-search/jcr_content/main-pars/image/visual-reverse-image-search-v2_intro.jpg "butterfly"

Image link: 
[![google](https://yt3.ggpht.com/a/AGF-l7-BBIcC888A2qYc3rB44rST01IEYDG3uzbU_A=s900-c-k-c0xffffffff-no-rj-mo)](https://google.com "google")
</pre>
Inline-style: 
![alt text](https://www.w3schools.com/w3css/img_lights.jpg "aurora")

Reference-style:
![alt text][img]

[img]: https://helpx.adobe.com/content/dam/help/en/stock/how-to/visual-reverse-image-search/jcr_content/main-pars/image/visual-reverse-image-search-v2_intro.jpg "butterfly"

Image link: 
[![google](https://yt3.ggpht.com/a/AGF-l7-BBIcC888A2qYc3rB44rST01IEYDG3uzbU_A=s900-c-k-c0xffffffff-no-rj-mo)](https://google.com "google")


## Blockquotes
<pre>
> blockquotes
>> nested blockquotes
</pre>
> blockquotes
>> nested blockquotes


## Code
<pre>
Inline code:
`backticks`

code block:
```javascript
console.log("Hello, world!")
```
</pre>
Inline code:
`backticks`

code block:
```javascript
console.log("Hello, world!")
```

## Table
id | attr1 | attr2
----|-------|------
00 | val01 | val02
01 | val11 | val12