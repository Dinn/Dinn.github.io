---
title: "마크다운 사용법 / Markdown Tutorial"
excerpt: "Headers, Emphasis, Lists, Links, Images, Blockquotes, Code, Table, Horizontal Rules, Task Lists, Footnotes, Definition Lists, Backslash Escapes"
date: 2019-11-18
last_modified_at: 2019-11-21T22:32:39

categories:
  - Dev Tips
tags:
  - Markdown
  - tutorial
  - cheatsheet
  - 마크다운
---

## Headers
```
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
```

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
```
Italic: 
*asterisks* or _underscores_

Bold: 
**asterisks** or __underscores__

Strikethrough: 
~~tildes~~

Bold and Italic: 

***asterisks or underscores***

**_asterisks or underscores_**

__*asterisks or underscores*__

___asterisks or underscores___
```

Italic: 
*asterisks* or _underscores_

Bold: 
**asterisks** or __underscores__

Strikethrough: 
~~tildes~~

Bold and Italic: 

***asterisks or underscores***

**_asterisks or underscores_**

__*asterisks or underscores*__

___asterisks or underscores___



## Lists
### Unordered list
```
* asterisk
+ plus
- minus
  * unordered sub-list
    + unordered sub-sub-list
    - unordered sub-sub-list
```
* asterisks
+ plus
- minus
  * unordered sub-list
    + unordered sub-sub-list
    - unordered sub-sub-list
    
### ordered list
```
1. A
1. B
    * mixed list1
    * mixed list2
1. C
    1. C1
    1. C2
```
1. A
1. B
    * mixed list1
    * mixed list2
1. C
    1. C1
    1. C2


## Links
```
[inline link](https://dinn.github.io)

[inline link with title](https://dinn.github.com "저녁밥 블로그")

[relative reference to a repository file](../docs/_docs/01-quick-start-guide.md)

[referenced link][reference text]

[number reference][1]

[text itself]

[reference text]: https://google.com "Google"
[1]: https://developer.mozilla.org
[text itself]: https://github.com
```
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
```
Inline-style: 
![alt text](https://www.w3schools.com/w3css/img_lights.jpg "aurora")

Reference-style:
![alt text][img]

[img]: https://helpx.adobe.com/content/dam/help/en/stock/how-to/visual-reverse-image-search/jcr_content/main-pars/image/visual-reverse-image-search-v2_intro.jpg "butterfly"

Linking image: 
[![google](https://yt3.ggpht.com/a/AGF-l7-BBIcC888A2qYc3rB44rST01IEYDG3uzbU_A=s900-c-k-c0xffffffff-no-rj-mo)](https://google.com "google")
```
Inline-style: 
![alt text](https://www.w3schools.com/w3css/img_lights.jpg "aurora")

Reference-style:
![alt text][img]

[img]: https://helpx.adobe.com/content/dam/help/en/stock/how-to/visual-reverse-image-search/jcr_content/main-pars/image/visual-reverse-image-search-v2_intro.jpg "butterfly"

Linking image: 
[![google](https://yt3.ggpht.com/a/AGF-l7-BBIcC888A2qYc3rB44rST01IEYDG3uzbU_A=s900-c-k-c0xffffffff-no-rj-mo)](https://google.com "google")


## Blockquotes
```
> blockquotes
>> nested blockquotes
```
> blockquotes
>> nested blockquotes


## Code
<pre style= "
  font-size: 0.8em;
  background-color: #263238;
  color: #fff;
  padding: 16.5px;
  border-radius: 4px;
  ">
Inline code:
`backquote`

Fenced code block:
```javascript
console.log("Hello, world!")
```
</pre>

Inline code:
`backquote`

Fenced code block:
```javascript
console.log("Hello, world!")
```

## Table
```
attr1 | attr2 | attr3
------ | ------ | ------
001 | face | article
002 | coffee | delicate

There must be three hyphens(-) seperating each header cell.

| header1 | header2 | header3 |
| :------ | :-----: | ------: |
| left-aligned | center-aligned | right-aligned |
|*inline markdown* | __is still__ | `available` |
```

attr1 | attr2 | attr3
------ | ------ | ------
001 | face | article
002 | coffee | delicate

There must be at least three hyphens(-) seperating each header cell.

| header1 | header2 | header3 |
| :------ | :-----: | ------: |
| left-aligned | center-aligned | right-aligned |
|*inline markdown* | __is still__ | `available` |


## Horizontal Rules
```
Three or more ...

* * *
***
asterisks

- - -
---
hypens

_ _ _
___
undersoces
```
Three or more ...

* * *
***
asterisks

- - -
---
hypens

_ _ _
___
undersoces


## Task Lists
```
- [x] The Godfather
- [x] Inception
- [ ] The Shawshank Redemption
- [ ] The Silence of the Lambs
- [x] Joker
- [ ] Forrest Gump
```
- [x] The Godfather
- [x] Inception
- [ ] The Shawshank Redemption
- [ ] The Silence of the Lambs
- [x] Joker
- [ ] Forrest Gump


## Footnotes
```
How to make footnote[^1]

Rules for identifier[^rule]

Other tips[^2]

[^1]: Add carat and identifier inside brackets.
[^rule]: Only contains alphanumeric characters(a-z, A-Z, 0-9) without spaces or tabs.
[^2]: Order of footnote doesn't follow numbers in bracket.
    
    Indent helps footnote to have multiple lines.
```
How to make footnote[^1]

Rules for identifier[^rule]

Other tips[^2]

[^1]: Add carat and identifier inside brackets.
[^rule]: Only contains alphanumeric characters(a-z, A-Z, 0-9) without spaces or tabs.
[^2]: Order of footnote doesn't follow numbers in bracket.
    
    Indent helps footnote to have multiple lines.


## Definition Lists
```
term1
 : term1 definition

term2
 : term2 definition
 : another definition
```
term1
 : term1 definition

term2
 : term2 definition
 : another definition


## Backslash Escapes
```
\\ backslash

\` backtick

\* asterisk

\_ underscore

\{\} curly braces

\[\] square brackets

\(\) paretheses

\# hash mark

\+ plus sign

\- minus sign(hyphen)

\. dot

\! exclamtion mark
```
\\ backslash

\` backtick

\* asterisk

\_ underscore

\{\} curly braces

\[\] square brackets

\(\) paretheses

\# hash mark

\+ plus sign

\- minus sign(hyphen)

\. dot

\! exclamtion mark

---