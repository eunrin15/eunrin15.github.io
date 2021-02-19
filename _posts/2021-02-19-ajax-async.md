---
title:  "[Ajax] async "
excerpt: "Ajax async 정리"

categories:
  - Ajax
tags:
  - [Ajax, async]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-19
last_modified_at: 2021-02-19
---

### 문제점
---
ajax 요청 시 로딩 이미지를 출력하지 않지만 client가 중복으로 request하는 것을 방지하고 싶었다.

### 해결방법
---
ajax 세팅 옵션에 async : false를 추가합니다.(동기식 방식으로 변경된다.)<br>
jQuery의 Ajax호출은 async: true가 기본이며, 이 속성을 기입하지 않는다면 기본적으로 비동기식으로 동작하게 됩니다.<br>
동기로 처리하게되면 버튼을 눌러 request 요청을 날렸을 때 response 요청이 올 때까지 다른 request 요청은 받지 않게 되어 중복호출을 방지합니다.

```javascript
async : false;
```

### 다른 해결방법
---