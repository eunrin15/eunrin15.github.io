---
title:  "[Spring] jQuery Ajax 심화"
excerpt: "jQuery Ajax 심화 개념 정리"

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

### ajax함수의 jqXHR(XMLHttpRquest)
---
jQuery 1.5부터 jQuery의 모든 ajax함수는 XMLHttpRequest객체의 상위 집합을 리턴받을 수 있다.<br>
이 객체를 jQuery에서는 jqXHR(응답값, 통신 status, readyStatus)이라 부르며, jqXHR의 함수로 콜백함수를 정의한다.

###  jqXHR(XMLHttpRquest)의 callback 함수
---
사용자 정의에 의해 순차적으로 실행<br>
Ajax에서 request를 리턴받아 호출 가능<br>

|함수명|설명|
|:----:|:----:|
|jqXHR.done(function( data, textStatus, jqXHR ) {});|성공시 호출되는 콜백함수|
|jqXHR.fail(function( jqXHR, textStatus, errorThrown ) {});|실패시 호출되는 콜백함수|
|jqXHR.always(function( data or jqXHR, textStatus, jqXHR' or errorThrown ) { });|항상 호출되는 콜백함수|

### Ajax 내부 callback과 jqXHR(XMLHttpRquest) callback의 실행순서
---
- 성공 : success(내부) > complete (내부) > done(jqXHR) > always(jqXHR)
-  실패 : error (내부) > complete (내부) > fail(jqXHR) > always(jqXHR)

###  callback함수 예제
---
- jqXHR(XMLHttpRquest) callback함수 예제1<br>
여러 개의 데이터를 전달하며 호출 후 콜백함수로 서버에서 값을 받는 예제.<br>
example02.do을 호출하며<br>
name, location을 요청데이터로 전달<br>
성공 시에 done 콜백함수를 호출

```javascript
$.ajax({
    url : "<c:url value='/example02.do'/>",
    data : {
    name : "gil-dong",
    location : "seoul"
    },
    })
    .done(function( data ) {
    if ( console && console.log ) {
    console.log( "Sample of data:", data.slice( 0, 100 ) );
    }
});
```

- jqXHR(XMLHttpRquest) callback함수 예제2<br>
example03.do를 호출하며 성공 시 done 콜백함수, 실패 시 fail 콜백함수 호출.<br>
성공, 실패여부에 상관없이 always 콜백함수는 항상 호출<br>
done, fail, always콜백함수는 ajax함수를 통해 리턴되어 request로 호출가능

```javascript
var jqxhr = $.ajax( "<c:url value='/example03.do'/>", )
    .done(function() {
    alert( "success" );
    })
    .fail(function() {
    alert( "error" );
    })
    .always(function() {
    alert( "complete" );
    });
    jqxhr.always(function() {
    alert( "second complete" );
});
```

### jQuery.get() - get함수
---
jQuery 1.5부터 success 콜백함수는 jqXHR (XMLHttpRequest의 상위 집합 객체)를 받을 수 있지만,<br>
JSONP와 같은 cross-domain request의 GET요청 시에는 jqXHR을 사용하여도 XHR인자는 success함수에서 undefined로 인식된다.<br>
jQuery.get()은 ajax를 GET요청하는 함수이며 jqXHR을 반환받는다.<br>
따라서 $.ajax()와 동일하게 done, fail, always 콜백함수를 쓸 수 있다.<br>
get함수는 ajax함수로 나타내면 다음과 같다.

```javascript
$.ajax({
    url: url,
    data: data,
    success: success,
    dataType: dataType
});
```

### get함수 설정
---

|설정|설명|default|type|
|:----:|:----:|:----:|:----:|
|url|request를 전달할 url명 |N/A|url String|
|data|request에 담아 전달할 data명과 data값|N/A|String/Plain Object|
|dataType|서버로부터 전달받을 데이터 타입|xml, json, script, or html|String|
|success|요청에 성공할 경우 호출되는 함수|N/A|Function( PlainObject data, String textStatus, jqXHR jqXHR )|


### jQuery.get() 예제
---
- url만 호출하고 결과값은 무시하는 경우

```javascript
$.get( "example.do" );
```

- url로 데이터만 보내고 결과는 무시하는 경우

```javascript
$.get( "example.do", { name: "gil-dong", location: "seoul" } );
```

- url을 호출하고 결과값을 Alert창으로 띄우는 경우

```javascript
$.get( " example.do", function( data ) {
    alert( "Data Loaded: " + data );
});
```

- url로 데이터를 보내고 결과값을 Alert창으로 띄우는 경우

```javascript
$.get( "example.do", { name: "gil-dong", location: "seoul" } )
    .done(function( data ) {
    alert( "Data Loaded: " + data );
});
```

### jQuery.post() - post함수
---
jQuery.post()은 ajax를 POST로 요청하는 함수로서 jqXHR을 반환받는다.<br>
따라서 ajax(), get()와 동일하게 done, fail, always콜백함수를 쓸 수 있다.<br>
jQuery.post 함수 설정은 get함수와 동일하다.(jQuery.post( url [, data ] [, success ] [, dataType ] ))<br>
jQuery.post함수를 ajax함수로 쓰면 다음과 같다.<br>

```javascript
$.ajax({
    type: "POST",
    url: url,
    data: data,
    success: success,
    dataType: dataType
});
```

### jQuery.POST() 예제
---
- url만 호출하고 결과값은 무시하는 경우

```javascript
$.post( "example.do" );
```

- url로 데이터만 보내고 결과는 무시하는 경우

```javascript
$.post( "example.do", { name: "gil-dong", location: "seoul" } );
```

- url을 호출하고 결과값을 console log를 남기는 경우

```javascript
$.post( "example.do", function( data ) {
    console.log( data.name );
    console.log( data.location );
});
```

- url로 데이터를 보내고 결과값을 Alert창으로 띄우는 경우

```javascript
$.post( "example.do", { name: "gil-dong", location: "seoul" } )
    .done(function( data ) {
    alert( "Data Loaded: " + data );
});
```