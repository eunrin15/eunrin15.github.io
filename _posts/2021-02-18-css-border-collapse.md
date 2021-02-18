---
title:  "[CSS] table 셀들 사이 간격 없애기"
excerpt: "border-collapse 설명 및 사용법"

categories:
  - CSS
tags:
  - [CSS, table, border-collapse]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-18
last_modified_at: 2021-02-18
---

### 문제점
---
table에서 표의 테두리 및 셀들 사이에 여백을 없애고 싶었다.

### 해결방법
---

```css
tr{
  border-collapse: collapse;
  border-spacing: 0;
}
```

### 설명
---
border-collapse속성은 간격을 설정할 수 있습니다.<br>
속성값이 separate이라면 간격의 크기는 border-spacing 속성으로 설정할 수 있습니다.

### 문법
---

```css
border-collapse: separate | collapse | initial | inherit
```

1. separate<br>
표(table)의 테두리와 셀(td)의 테두리 사이에 간격을 둡니다.
2. collapse<br>
표(table)의 테두리와 셀(td)의 테두리 사이의 간격을 없앱니다.<br>
겹치는 부분은 한 줄로 나타냅니다.
3. initial<br>
기본값으로 설정합니다.
4. inherit<br>
부모 요소의 속성값을 상속받습니다.