---
title:  "[jQuery] $.each 반복문"
excerpt: "$.each 반복문 분석"

categories:
  - jQuery
tags:
  - [jQuery, each()]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-18
last_modified_at: 2021-02-18
---

### each()
---

```javascript
$.each(object, function(index, item){ });
```

object와 배열 모두 사용할 수 있는 일반적인 반복 함수입니다.<br>
배열과 length 속성을 갖는 유사 배열 객체들을 index만큼 반복할 수 있습니다.<br>

```javascript
var arr = [
    { name : '홍길동', age : '20'}
    { name : '김길동', age : '21'}        
];

$.each(arr, function(index, item){
    var result = index + "/" + item.name + "/" + item.age;
    console.log(result);
})
```

```
[실행 결과]
0/홍길동/20
1/김길동/21
```

1. $.each 메서드의 첫 번째 매개변수로 arr이라는 배열을 사용했습니다.
2. 두 번째 매개변수는 콜백 함수인데 첫 번째 index는 배열의 인덱스 또는 객체의 키를 의미하고 두 번째 매개 변수 item은 해당 인덱스나 키가 가진 값을 의미합니다.