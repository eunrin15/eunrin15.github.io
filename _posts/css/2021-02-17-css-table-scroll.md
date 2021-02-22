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
tbody {
    display: block;
    height: 100px;
    overflow: auto;
}
thead, tbody tr {
    display: table;
    width: 100%;
    table-layout: fixed;
}
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```