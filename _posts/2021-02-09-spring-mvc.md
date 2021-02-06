---
title:  "[Spring]MVC"
excerpt: "MVC 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, maven]

toc: true
toc_sticky: true
 
date: 2021-02-09
last_modified_at: 2021-02-09
---

### 구성요소
---
MVC(Mode-View-Controller)패턴은 코드를 기능에따라 Mode, View, Controller 3가지 요소로 분리한다.
1. Model<br>어플리케이션의 데이터와 비즈니스 로직을 담는 객체이다.
2. View<br>Model의 정보를 사용자에게 표시한다.<br>하나의 Model을 다양한 View에서 사용할 수 있다.
3. Controller<br>Model과 View의 중계역할.<br>사용자의 요청을 받아 Model에 변경된 상태를 반영하고, 응답을 위한 View를 선택한다.

MVC 패턴은 UI 코드와 비즈니스 코드를 분리함으로써 종속성을 줄이고, 재사용성을 높이며, 보다 쉬운 변경이 가능하도록 한다.

### Spring MVC
---
DispatcherServlet, HandlerMapping, Controller, Interceptor, ViewResolver, View등 각 컴포넌트들의 역할이 명확하게 분리된다.<br>
HandlerMapping, Controller, View등 컴포넌트들에 다양한 인터페이스 및 구현 클래스를 제공한다.<br>
Controller(@MVC)나 폼 클래스(커맨드 클래스) 작성시에 특정 클래스를 상속받거나 참조할 필요 없이 POJO 나 POJOstyle의 클래스를 작성함으로써 비지니스 로직에 집중한 코드를 작성할 수 있다.<br>
웹요청 파라미터와 커맨드 클래스간에 데이터 매핑 기능을 제공한다.<br>
데이터 검증을 할 수 있는, Validator와 Error 처리 기능을 제공한다.<br>
JSP Form을 쉽게 구성하도록 Tag를 제공한다.<br>

### Spring MVC의 핵심 컴포넌트
---
1. DispatcherServlet<br>Spring MVC Framework의 Front Controller, 웹요청과 응답의 Life Cycle을 주관한다.
2. HandlerMapping<br>웹요청시 해당 URL을 어떤 Controller가 처리할지 결정한다.
3. Controller<br>비지니스 로직을 수행하고 결과 데이터를 ModelAndView에 반영한다.
4. ModelAndView<br>Controller가 수행 결과를 반영하는 Model 데이터 객체와 이동할 페이지 정보(또는 View객체)로 이루어져 있다.
5. ViewResolver<br>어떤 View를 선택할지 결정한다.
6. View<br>결과 데이터인 Model 객체를 display한다.

### Spring MVC 컴포넌트 간의 관계와 흐름
---
Client의 요청이 들어오면 DispatchServlet이 가장 먼저 요청을 받는다.<br>
HandlerMapping이 요청에 해당하는 Controller를 return한다.<br>
Controller는 비지니스 로직을 수행(호출)하고 결과 데이터를 ModelAndView에 반영하여 return한다.<br>
ViewResolver는 view name을 받아 해당하는 View 객체를 return한다.<br>
View는 Model 객체를 받아 rendering한다.<br>
![MVC컴포넌트 간의 관계와 흐름](/imgsrc/Spring_MVC_Component.jpg)

### DispatcherServlet
---
Controller로 향하는 모든 웹요청의 진입점이며, 웹요청을 처리하며, 결과 데이터를 Client에게 응답 한다.<br>
Spring MVC의 웹요청 Life Cycle을 주관<br>
![MVC컴포넌트 간의 관계와 흐름](/imgsrc/Spring_MVC_DispatcherServlet.jpg)

### DispatcherServlet, ApplicationContext, WebApplicationContext
---
하나의 빈 설정파일에 모든 빈을 등록할 수도 있지만, 아래와 같이 Layer 별로 빈파일을 나누어 등록하고 ApplicationContext, WebApplicationContext 사용하는것을 권장.<br>
ApplicationContext : ContextLoaderListener에 의해 생성. persistance, service layer의 빈<br>
WebApplicationContext : DispatcherServlet에 의해 생성. presentation layer의 빈<br>
ContextLoaderListener는 웹 어플리케이션이 시작되는 시점에 ApplicationContext을 만들고, 이 ApplicationContext의 빈<br>
![MVC컴포넌트 간의 관계와 흐름](/imgsrc/Spring_MVC_DAW.jpg)


