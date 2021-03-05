---
title:  "[JS] 해당 함수가 발생한 이벤트 정보를 알아내는 방법"
excerpt: "해당 함수가 발생한 이벤트 정보를 알아내는 방법 정리"

categories:
  - Javascript
tags:
  - [Javascript, event]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-03-05
last_modified_at: 2021-03-05
---

### 문제점
---
특정 Element의 이벤트를 통한 함수 사용 시 이벤트 정보를 이용하고 싶었다.

### 해결방법
---
우선 아래와 같이 Element의 이벤트 함수에 event를 매개변수로 전달해줍니다.

```html
<input type="button" onclick="sampleFunction(event)" ... >
```

그 다음 Javascript에서 event 매개변수를 이용하면 됩니다.

```javascript
function sampleFunction(event){
    console.log(event)  //console창에서 이벤트의 정보를 확인할 수 있습니다.

    alert(event.target.id); //해당 이벤트의 target란의 id값을 알림창으로 띄우는 예시입니다.
}
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```