---
title:  "[Spring] ajax로 controller에 데이터 송신"
excerpt: "Ajax로 jsp에서 controller로 데이터 송신"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, ajax, controller, jsp]

toc: true
classes: wide

sidebar:
  nav: "docs"

date: 2021-02-15
last_modified_at: 2021-02-15
---

### jsp ajax
---

```javascript
function testFunction(){
    var url = "<c:url value='/test.do'/>";
    var paramInfo = {}; // data를 따로 써줘도 상관없다. 묶어서 보내기 위해서 이렇게 사용함
    paramInfo.data = "data"; // 예시

    $.ajax({
        type: "POST",
        data : paramInfo,
        dataType: "json", 
        url: url, 
        headers: {"AJAX": "true"}, 
        success: function (result) {
            //...
        }
    });
}
```

### controller
---

```java
@RequestMapping("/test.do")
public void testFunction(Locale locale, HttpServletRequest request, ModelMap model, HttpSession session) throws Exception {
    //request로 받아서 사용
}
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```