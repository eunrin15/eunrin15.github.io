---
title:  "[Spring] Pagination Tag"
excerpt: "Pagination Tag 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, Pagination]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-22
last_modified_at: 2021-02-22
---

### Pagination Tag이란?
---

![Spring_Paginationtag](/imgsrc/Spring_Paginationtag.JPG)

페이징 처리의 편의를 위해 전자정부프레임워크에서 제공하는 태그입니다.<br>
페이징 기능을 사용할때 기능은 유사하지만 이미지나 라벨등의 포맷만 다양하게 사용하게 되는 경우가 있다.
따라서 포맷별로 렌더링할 클래스를 빈설정 파일에 설정하고 태그에서 입력된 정보를 기반으로 어떤 렌더링 클래스를 사용할지 결정하는 방식으로 동작한다.
PaginationRenderer는 포맷에 따라 페이징을 렌더링하는 역활을 담당하고, PaginationManager는 어떤 PaginationRenderer를 사용할지를 담당한다.
렌더링에 필요한 데이터는 PaginationInfo에 담겨 있다.

### PaginationInfo
---
페이징 처리를 위한 데이터들을 담고 있는 빈 클래스입니다.
Tag 클래스에서 여기 담긴 정보를 기반으로 페이징을 렌더링합니다.
사용자입력여부가 yes인 프로퍼티들은 Controller에서 직접 해당 setter에 값을 넣어줘야 하며, no인 프로퍼티인 값들은 사용자가 입력한 다른 프로퍼티 값으로 자동계산되는 프로퍼티들입니다.

[PaginationInfo의 기본 프로퍼티(필드)]

|이름|설명|사용자입력여부|계산공식|
|:----:|:----|:----:|:----|
|currentPageNo|현재 페이지 번호|yes||
|recordCountPerPage|한 페이지당 게시되는 게시물 건 수|yes||
|pageSize|페이지 리스트에 게시되는 페이지 건수|yes||
|totalRecordCount|전체 게시물 건 수|yes||
|totalPageCount|페이지 개수|no|totalPageCount = ((totalRecordCount-1)/recordCountPerPage) + 1|
|firstPageNoOnPageList|페이지 리스트의 첫 페이지 번호|no|firstPageNoOnPageList = ((currentPageNo-1)/pageSize)*pageSize + 1|
|lastPageNoOnPageList|페이지 리스트의 마지막 페이지 번호|no|lastPageNoOnPageList = firstPageNoOnPageList+pageSize-1<br>if(lastPageNoOnPageList>totalRecordCount){lastPageNoOnPageList=totalPageCount}|
|firstRecordIndex|페이징 SQL의 조건절에 사용되는 시작 rownum|no|firstRecordIndex = (currentPageNo - 1) * recordCountPerPage|
|lastRecordIndex|페이징 SQL의 조건절에 사용되는 마지막 rownum|no|lastRecordIndex = currentPageNo * recordCountPerPage|

### PaginationTag
---
[페이징을 위한 Tag class인 PaginationTag에서 커스텀 태그를 통해 사용자로 부터 입력 받는 프로퍼티]

|이름|설명|필수여부|
|:----|:----|:----:|
|paginationInfo|페이징리스트를 만들기 위해 필요한 데이터.<br>데이터 타입은 egovframework.rte.ptl.mvc.tags.ui.pagination.PaginationInfo이다.|yes|
|type|페이징리스트 렌더링을 담당할 클래스의 아이디.<br>이 아이디는 빈설정 파일에 선언된 프로퍼티 rendererType의 key값이다.	|yes|
|jsFunction|페이지 번호에 걸리게 될 자바스크립트 함수 이름.<br>페이지 번호가 기본적인 argument로 전달된다.|yes|

[PaginationTag의 주요 로직]
- 어떤 PaginationRenderer를 사용할지 PaginationManager에게 위임한다.
- 실제 페이징을 위한 작업은 PaginationManager가 반환한 PaginationRenderer이 담당한다.
- PaginationRenderer가 반환한 String 데이터를 출력한다.

```java
//빈 설정 정보와 type 프로퍼티값으로 PaginationRenderer 구현 클래스 반환.
PaginationRenderer paginationRenderer = paginationManager.getRendererType(type);
//페이징 Contents 반환.
String contents = paginationRenderer.renderPagination(paginationInfo, jsFunction);
//출력.
out.println(contents);
```

### PaginationManager
---

### PaginationRenderer
---
PaginationInfo의 데이터를 기반으로 페이징을 렌더링하는 역활을 담당합니다.


```
[참고한 사이트]
https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte:ptl:view:paginationtag
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```