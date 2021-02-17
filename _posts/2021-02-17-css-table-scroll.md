---
title:  "[CSS] table tbody 스크롤바"
excerpt: "table tbody 스크롤바 제작"

categories:
  - CSS
tags:
  - [CSS, table, scroll, overflow]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-17
last_modified_at: 2021-02-17
---

### 문제점
---
table에서 thead는 고정된 채로 tbody의 내용들만 스크롤해서 보고 싶었다.

### 해결방법
---

```css
table, tr td {
    border: 1px solid red
}
tbody {
    display: block;
    height: 50px;
    overflow: auto;
}
thead, tbody tr {
    display: table;
    width: 100%;
    table-layout: fixed;
}
```