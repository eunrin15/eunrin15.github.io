---
title:  "[Spring] Dispatcher-Servlet"
excerpt: "Dispatcher-Servlet 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, Dispatcher-Servlet]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-19
last_modified_at: 2021-02-19
---

### Dispatcher-Servlet란?
---
Servlet Container에서 HTTP프로토콜을 통해 들어오는 모든 요청을 프레젠테이션 계층의 제일앞에 둬서 중앙집중식으로 처리해주는 프론트 컨트롤러(Front Controller)입니다.<br>
클라이언트로부터 요청이 들어오면 Tomcat과 같은 서블릿 컨테이너가 요청을 받습니다.<br>
이 때 제일 앞에서 서버로 들어오는 모든 요청을 처리하는 프론트 컨트롤러를 Spring에서 정의한 것을 Dispatcher-Servlet이라고 합니다.<br>
그래서 공통처리 작업을 Dispatcher-Servlet이 처리한 후에 적절한 세부 컨트롤러로 작업을 위임해 줍니다.<br>
작업을 위임할 수 있게 Dispatcher-Servlet이 처리하는 url 패턴을 지정해주어야 하는데 일반적으로는 /*.do와 같은 /로 시작하며 .do로 끝나는 url 패턴에 대해서 처리하라고 지정해줍니다.<br>

![Spring_Dispatcher_Servlet_Flow](/imgsrc/Spring_Dispatcher_Servlet_Flow.png)

1. 클라이언트로부터 요청(Request) 접수<br>
서블릿 컨테이너에서 받은 HTTP요청을 Dispatcherservlet에 할당해주는데 이를 먼저 web.xml에서 설정해준다.<br>
web.xml에서 서블릿 설정과 url매핑을 설정하며, 요청이 들어오면 Dispatcherservlet에서 요청을 접수한다.

2. Dispatcherservlet에서 컨트롤러(Controller)로 위임<br>
위의 url매핑대로 요청이 접수되면 Dispatcherservlet은 위의 MVC 흐름 그림처럼 Handler Mapping을 통해 해당 요청을 알맞은 컨트롤러로 위임한다.<br>
Handler는 url요청을 다루는 녀석으로 요청url과 컨트롤러를 매핑하는 역할을 한다.

3. 컨트롤러(Controller)의 모델 생성<br>
HandlerMapping을 통해 요청을 위임받은 컨트롤러(Controller)는 필요한 비즈니스 로직을 호출/수행하여 처리 결과(모델,M)를 생성하고 이 모델(M)과 출력될 뷰(View)를 Dispatcherservlet에 반환해준다.

4. ViewResolver<br>
컨트롤러(Controller)로 부터 ModelAndView 정보를 전달받은 Dispatcherservlet은 ViewResolver란 클래스를 이용하여 사용자에게 출력할 View 객체를 얻게 된다.

5. View<br>
ViewResolver를 통해 얻은 View객체를 통해 사용자에게 보여줄 화면을 출력한다.

### 관련 포스팅
---
[**web.xml**](https://eunrin15.github.io/spring/spring-webxml)