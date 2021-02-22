---
title:  "[Spring] ServletRequest"
excerpt: "ServletRequest 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, ServletRequest]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-22
last_modified_at: 2021-02-22
---

### ServletRequest란?
---
클라이언트의 요청 정보를 서블릿으로 넘겨주기 위한 객체이다. 즉, 요청에 대한 정보를 가진 객체이다.
서블릿 컨테이너가 ServletRequest를 생성하고 객체를 서블릿의 service 메서드의 파라미터로 전달한다.

ServletRequest에는 parameter의 이름과 값, 속성, inputStream과 같은 데이터를 제공한다.
HTTP Protocol에서 존재하는 추가적인 데이터는 ServletRequest를 상속한 HttpServletRequest에서 제공한다.

### 메소드
---
