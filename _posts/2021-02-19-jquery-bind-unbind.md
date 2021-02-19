---
title:  "[jQuery] bind와 unbind"
excerpt: "jQuery bind와 unbind 정리"

categories:
  - jQuery
tags:
  - [jQuery, bind, unbind]

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
버튼에 클릭 이벤트를 jQuery.bind(), jQuery.unbind() 로 처리해 클릭 이벤트를 bind 시켜놓는다.(클릭의 비활성화)<br>
한 번 클릭시 클릭 이벤트를 unbind 시키고, ajax 요청이 끝나면 다시 bind 시킨다.

```javascript
$(document).ready(function() {
	$('#foo').bind('click', function() {
		doSomething();  
	});
});
 
var doSomething = function(){
	$('#foo').unbind('click');
  
	$.ajax({
		type: "POST",
		url: "some.do"
	}).done(function() {
		$('#foo').bind('click', function() {
			doSomething();  
		});    
	});
}
```

### 다른 해결방법
---
[**ajax의 async**](https://eunrin15.github.io/ajax/ajax-async)