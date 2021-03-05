---
title:  "[jQuery] eventHandler 추가 및 제거 방법"
excerpt: "jQuery를 이용한 eventHandler 추가 및 제거 방법 정리"

categories:
  - jQuery
tags:
  - [jQuery, event, eventHandler]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-03-05
last_modified_at: 2021-03-05
---

### 문제점
---
자바스크립트에서 특정 Element의 eventHandler를 변경해주고 싶다.


### 해결 방법
---

```javascript
[추가]
1.
function sampleFunction(){
    ...
}
document.getElementById("ID Value").addEventListener("이벤트 속성", sampleFunction);

2.
document.getElementById("ID Value").addEventListener("이벤트 속성", function(){
    ...
});

[제거]
addEventListener를 removeEventListener으로 변경해주면 됩니다.
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```