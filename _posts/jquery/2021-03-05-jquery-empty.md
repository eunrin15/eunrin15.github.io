---
title:  "[jQuery] 해당 태그를 남긴 채 하위요소 한 번에 지우는 방법"
excerpt: "jQuery를 이용한 해당 태그를 남긴 채 하위요소 한 번에 지우는 방법 정리"

categories:
  - jQuery
tags:
  - [jQuery, empty]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-03-05
last_modified_at: 2021-03-05
---

### 문제점
---
특정 태그의 하위 구성요소를 한 번에 지우고 싶었다.(ex: Table의 값..)

### 해결방법
---
특정 태그의 하위 요소를 한 번에 삭제하는 방법입니다.<br>
다음 방법으로 하면 특정 태그는 남아 있습니다.

```javascript
$("특정 태그의 id, class, name...").empty();
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```