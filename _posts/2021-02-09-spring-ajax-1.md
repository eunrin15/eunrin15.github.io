---
title:  "[Spring] jQuery Ajax 기본"
excerpt: "jQuery Ajax 개념 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, ajax]

toc: true
classes: wide

sidebar:
  nav: "docs"

date: 2021-02-09
last_modified_at: 2021-02-09
---

jQuery는 브라우저 호환성을 제공하는 자바스크립트 라이브러리.<br>
jQuery는 자바스크립트 프레임워크로서 간결한 문장의 표현으로 front-end(화면) 개발시 생산성 향상.<br>
다양한 오픈소스 라이브러리들을 통해 java script로 ajax, json parser, css selector, dom seletor, event 등 다양한 ui 기능 등을 제공하고 front-end(화면)을 동적으로 제어가능

### jQuery 설정
---
- jQuery url을 직접 명시<br>

```javascript
<script src="https://code.jquery.com/jquery-1.11.0.min.js"></script>
```

- jQuery script를 직접 추가하여 참조<br>
 jquery-버전.js를 다운받아 프로젝트 하위 경로에 추가한 후, 저장한 경로를 설정.<br>

```javascript
<script type="text/javascript" src="jQuery파일 경로"></script>
```

### jQuery.ajax(url[,settings]) 함수
---
jQuery Ajax기능을 위해서는 기본적으로 jQuery.ajax(url[,settings]) 함수를 이용<br>

|설정|설명|default|type|
|:----:|:----:|:----:|:----:|
|type|http 요청 방식 설정(POST, GET, PUT.DELETE…)|GET|type string|
|url|request를 전달할 url명|N/A|url string|
|data|request에 담아 전달할 data명과 data값|N/A|String/Plain Object/Array|
|contentType|server로 데이터를 전달할 때 contentType|'application/x-www-form-urlencoded; charset=UTF-8'|contentType String|
|dataType|서버로부터 전달받을 데이터 타입|xml, json, script, or html|xml/html/script/json/jsonp/multiple,space-separated values|
|statusCode|HTTP 상태코드에 따라 분기처리되는 함수|N/A|상태코드로 분리되는 함수|
|beforeSend|request가 서버로 전달되기 전에 호출되는 콜백함수|N/A|Function( jqXHR jqXHR, PlainObject settings )|
|error|요청을 실패할 경우 호출되는 함수|N/A|Function( jqXHR jqXHR, String textStatus, String errorThrown )|
|success|요청에 성공할 경우 호출되는 함수|N/A|Function( PlainObject data, String textStatus, jqXHR jqXHR )|
|crossDomain|crossDomain request(jsonP와 같은)를 강제할 때 설정(cross-domain request설정 필요)|same-domain request에서 false, cross-domain request에서는 true|Boolean|

### 기본 예제
---
하나의 파라미터를 Ajax request로 전달하는 예제.

```javascript
$.ajax({
  type : "POST",
  url : "<c:url value='/example01.do'/>",
  contentType: "application/x-www-form-urlennocoded; charset=UTF-8",
  dataType:'json',
  data : {
  sampleInput : "sampleData"
  },
  success : function(data, status, jqXHR) {
  // 통신이 정상적일때 해당 함수 실행
  },
  error : function(request, status, error){
  // 통신이 비정상적일때 해당 함수 실행
  },
  complete : function(jqXHR, status) {
  //통신의 성공과 실패시 해당 함수 실행
  }
});
```

Http 요청 동기Ajax Post Method 방식으로 example01.do로 호출<br>
contentType을 application/x-www-form-urlennocoded 방식으로 charset을 UTF-8 으로 설정.<br>
sampleInput이란 데이터명으로 “sampleData” String을 전달<br>
통신의 요청이 성공할 경우 success 함수 호출<br>
통신이 비정상적일때 error 함수 호출<br>
통신의 성공과 실패시 complete 함수 호출

