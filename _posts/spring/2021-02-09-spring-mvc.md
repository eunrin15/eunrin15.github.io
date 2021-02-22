---
title:  "[Spring] MVC"
excerpt: "MVC 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, MVC]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-09
last_modified_at: 2021-02-19
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
- DispatcherServlet, HandlerMapping, Controller, Interceptor, ViewResolver, View등 각 컴포넌트들의 역할이 명확하게 분리된다.<br>
- HandlerMapping, Controller, View등 컴포넌트들에 다양한 인터페이스 및 구현 클래스를 제공한다.<br>
- Controller(@MVC)나 폼 클래스(커맨드 클래스) 작성시에 특정 클래스를 상속받거나 참조할 필요 없이 POJO 나 POJOstyle의 클래스를 작성함으로써 비지니스 로직에 집중한 코드를 작성할 수 있다.<br>
- 웹요청 파라미터와 커맨드 클래스간에 데이터 매핑 기능을 제공한다.<br>
- 데이터 검증을 할 수 있는, Validator와 Error 처리 기능을 제공한다.<br>
- JSP Form을 쉽게 구성하도록 Tag를 제공한다.<br>

### Spring MVC의 핵심 컴포넌트
---
1. DispatcherServlet<br>
Spring MVC Framework의 Front Controller, 웹요청과 응답의 Life Cycle을 주관한다.
2. HandlerMapping<br>
웹요청시 해당 URL을 어떤 Controller가 처리할지 결정한다.
3. Controller<br>
비지니스 로직을 수행하고 결과 데이터를 ModelAndView에 반영한다.
4. ModelAndView<br>
Controller가 수행 결과를 반영하는 Model 데이터 객체와 이동할 페이지 정보(또는 View객체)로 이루어져 있다.
5. ViewResolver<br>
어떤 View를 선택할지 결정한다.
6. View<br>
결과 데이터인 Model 객체를 display한다.

### Spring MVC 컴포넌트 간의 관계와 흐름
---
- Client의 요청이 들어오면 DispatchServlet이 가장 먼저 요청을 받는다.<br>
- HandlerMapping이 요청에 해당하는 Controller를 return한다.<br>
- Controller는 비지니스 로직을 수행(호출)하고 결과 데이터를 ModelAndView에 반영하여 return한다.<br>
- ViewResolver는 view name을 받아 해당하는 View 객체를 return한다.<br>
- View는 Model 객체를 받아 rendering한다.<br>
![MVC컴포넌트 간의 관계와 흐름](/imgsrc/Spring_MVC_Component.jpg)

### DispatcherServlet
---
Controller로 향하는 모든 웹요청의 진입점이며, 웹요청을 처리하며, 결과 데이터를 Client에게 응답 한다.<br>
Spring MVC의 웹요청 Life Cycle을 주관<br>
이 때 DispatcherServlet이 모든 요청을 가로채는 건 아니고 [**web.xml**](https://eunrin15.github.io/spring/spring-webxml)에 등록된 내용만 가로챈다.<br>
최초의 web.xml 에서는 url-pattern이 '/'와 같이 해당 애플리케이션의 모든 URL로 등록돼있기 때문에, 만약 *. do와 같이 특정 URL만 적용하고 싶다면 url-pattern의 내용을 바꿔주어 범위를 변경하면 된다.<br>
![MVC컴포넌트 간의 관계와 흐름](/imgsrc/Spring_MVC_DispatcherServlet.jpg)

### DispatcherServlet, ApplicationContext, WebApplicationContext
---
- 하나의 빈 설정파일에 모든 빈을 등록할 수도 있지만, 아래와 같이 Layer 별로 빈파일을 나누어 등록하고 ApplicationContext, WebApplicationContext 사용하는것을 권장.<br>
- ApplicationContext : ContextLoaderListener에 의해 생성. persistance, service layer의 빈<br>
- WebApplicationContext : DispatcherServlet에 의해 생성. presentation layer의 빈<br>
- ContextLoaderListener는 웹 어플리케이션이 시작되는 시점에 ApplicationContext을 만들고, 이 ApplicationContext의 빈<br>
![DispatcherServlet, ApplicationContext, WebApplicationContext](/imgsrc/Spring_MVC_DAW.jpg)

### web.xml에 DispatcherServlet 설정
---
![web.xml에 DispatcherServlet 설정](/imgsrc/Spring_MVC_DispatcherServlet_Setting.jpg)

### @MVC
---
- 어노테이션을 이용한 설정 : XML 기반으로 설정하던 정보들을 어노테이션을 사용해서 정의한다.
- 유연해진 메소드 시그니쳐 : Controller 메소드의 파라미터와 리턴 타입을 좀 더 다양하게 필요에 따라 선택
할 수 있다.
- POJO-Style의 Controller : Controller 개발시에 특정 인터페이스를 구현 하거나 특정 클래스를 상속해야할
필요가 없다.<br>하지만, 폼 처리, 다중 액션등 기존의 계층형 Controller가 제공하던 기능들을 여전히 쉽게 구현
할 수 있다.
- Bean 설정파일 작성 : @Controller만 스캔하도록 설정한다.

### @MVC <context:component-scan/> 설정
---
- @Component, @Service, @Repository, @Controller 가 붙은 클래스들을 읽어들여 ApplicationContext,
WebApplicationContext에 빈정보를 저장, 관리한다.
- @Controller만 스캔하려면 <context:include-filter>나 <context:exclude-filter>를 사용해야 한다<br>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation=http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context-3.0.xsd>
<context:component-scan base-package="com.easycompany.controller.annotation">
<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
</context:component-scan>
</beans>
```

### HandlerMapping
---
RequestMappingHandlerMapping(DefaultAnnotationHandlerMapping는 deprecated)
- @MVC 개발을 위한 HandlerMapping. 표준프레임워크 3.0(Spring 3.2.9)에서 사용가능.
- 기존 DefaultAnnotationHandlerMapping이 deprecated되면서 대체됨.
- @RequestMapping에 지정된 url과 해당 Controller의 메소드 매핑
- RequestMappingHandlerMapping은 기본 HandlerMapping이며, 선언하기 위해서는 다음과 같이 세가지 방법이 있다.
- 선언하지 않는 방법 : 기본 HandlerMapping이므로 지정하지 않아도 사용가능하다.
- <mvc:annotation-driven/>을 선언하는 방법 : @MVC사용 시 필요한 빈들을 등록해주는 <mvc:annotation-driven/>을 설정하면 내부에서 RequestMappingHandlerMapping, RequestMappingHandlerAdapter 이 구성된다.
- RequestMappingHandlerMaping을 직접 선언하는 방법 : 다른 HandlerMapping과 함께 사용할 때 선언한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-2.5.xsd http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-2.5.xsd">
    <context:component-scan base-package="org.mycode.controller" />
    <!—명시적 선언-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
</beans>
```

SimpleUrlAnnotationHandlerMapping(표준프레임워크3.0부터deprecated됨 mvc 태그로 변경)
- DefaultAnnotationHandlerMapping은 특정 url에 대해 interceptor를 적용할수 없음. -> 확장 HandlerMapping
- DefaultAnnotationHandlerMapping과 함께 사용. (order 프로퍼티를 SimpleUrlAnnotationHandlerMapping에 준다.)<br>
![Spring_MVC_SimpleAnnotation](/imgsrc/Spring_MVC_SimpleAnnotation.JPG)

### @controller 관련 Annotation
---
1. @Controller<br>
해당 클래스가 Controller임을 나타내기 위한 어노테이션
2. @RequsetMapping<br>
요청에 대해 어떤 Controller, 어떤 메소드가 처리할지를 맵핑하기 위한 어노테이션
3. @RequestParam<br>
Controller 메소드의 파라미터와 웹 요청 파라미터와 맵핑하기 위한 어노테이션
4. @ModelAttribute<br>
Controller 메소드의 파라미터와 리턴값을 Model 객체와 바인딩하기 위한 어노테이션
5. SessionAttributes<br>
Model 객체를 세션에 저장하고 사용하기 위한 어노테이션
6. @CommandMap<br>
Controller 메소드의 파라미터를 Map형태로 받을 때 웹 요청 파라미터와 맵핑하기 위한 어노테이션

### @Controller
---
@MVC에서 Controller를 만들기 위해서는 작성한 클래스에 @Controller를 붙여주면 된다. 특정 클래스를 구현하거나 상속할 필요가 없다.

```java
import org.springframework.stereotype.Controller;

@Controller
public class HelloController {
...
}
```

### @RequestMapping
---
요청에 대해 어떤 Controller, 어떤 메소드가 처리할지를 매핑하기 위한 어노테이션이다.<br>
[ 관련속성 ]

|이름|타입|맵핑 조건|설명|
|:----:|:----:|:----:|:----:|
|value|String[]|URL 값|@RequestMapping(value=”/hello.do”)<br>@RequestMapping(value={”/hello.do”, ”/world.do” })<br>@RequestMapping(”/hello.do”)<br>Ant-Style 패턴매칭 이용 : ”/myPath/*.do”|
|method|Request Method[]|HTTP Request 메소드값|@RequestMapping(method = RequestMethod.POST)<br>사용 가능한 메소드 : GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE|
|params|String[]|HTTP Request 파라미터| params=“myParam=myValue” : HTTP Request URL중에 myParam이라는 파라미터가 있어야 하고 값은 myValue이어야 맵핑<br>params=“myParam” : 파라미터 이름만으로 조건을 부여<br>"!myParam" : myParam이라는 파라미터가 없는 요청 만을 맵핑<br>@RequestMapping(params={“myParam1=myValue”, “myParam2”, ”!myParam3”})와 같이 조건을 주었다면, HTTP Request에는 파라미터 myParam1이 myValue값을 가지고 있고, myParam2 파라미터가 있어야 하고, myParam3라는 파라미터는 없어야함

### @RequestMapping 설정
---
@RequestMapping은 클래스 단위(type level)나 메소드 단위(method level)로 설정할 수 있다.

- type level<br>
/hello.do 요청이 오면 HelloController의 hello 메소드가 수행된다.<br>
type level에서 URL을 정의하고 Controller에 메소드가 하나만 있어도 요청 처리를 담당할 메소드 위에 @RequestMapping 표기를 해야 제대로 맵핑이 된다.<br>

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/hello.do")
public class HelloController {

  @RequestMapping
  public String hello(){
  ...
  }
}
```

- method level<br>
/hello.do 요청이 오면 hello 메소드,<br>
/helloForm.do 요청은 GET 방식이면 helloGet 메소드, POST 방식이면 helloPost 메소드가 수행된다.<br>

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class HelloController {

  @RequestMapping(value="/hello.do")
  public String hello(){
  ...
  }

  @RequestMapping(value="/helloForm.do", method = RequestMethod.GET)
  public String helloGet(){
  ...
  }

  @RequestMapping(value="/helloForm.do", method = RequestMethod.POST)
  public String helloPost(){
  ...
  }
}
```

- type + method level<br>
type level, method level 둘 다 설정할 수도 있는데,<br>
이 경우엔 type level에 설정한 @RequestMapping의 value(URL)를 method level에서 재정의 할수 없다.<br>
/hello.do 요청시에 GET 방식이면 helloGet 메소드, POST 방식이면 helloPost 메소드가 수행된다. <br>

```java
@Controller
@RequestMapping("/hello.do")
public class HelloController {
  @RequestMapping(method = RequestMethod.GET)
  public String helloGet(){
  ...
  }

  @RequestMapping(method = RequestMethod.POST)
  public String helloPost(){
  ...
  }
}
```

### @RequestParam
---
@RequestParam은 Controller 메소드의 파라미터와 웹요청 파라미터와 맵핑하기 위한 어노테이션이다.<br>
[ 관련 속성 ]

|이름|타입|설명|
|:----:|:----:|:----:|
|value|String|파라미터 이름|
|required|boolean|해당 파라미터가 반드시 필수 인지 여부.<br>기본값은 true이다.|

해당 파라미터가 Request 객체 안에 없을때 그냥 null값을 바인드 하고 싶다면, 아래 예제의 pageNo 파라미터 처럼 required=false로 명시해야 한다.<br>
name 파라미터는 required가 true이므로, 만일 name 파라미터가 null이면 org.springframework.web.bind.MissingServletRequestParameterException이 발생한다.

```java
@Controller
public class HelloController {

  @RequestMapping("/hello.do")
  public String hello(@RequestParam("name") String name,
                  @RequestParam(value="pageNo", required=false) String pageNo){
  ...
  }
}
```

### @ModelAttribute
---
@ModelAttribute은 Controller에서 2가지 방법으로 사용된다.
1. Model 속성(attribute)과 메소드 파라미터의 바인딩.
2. 입력 폼에 필요한 참조 데이터(reference data) 작성. - SimpleFormContrller의 referenceData 메소드와 유사한 기능.

[ 관련 속성 ]

|이름|타입|설명|
|:----:|:----:|:----:|
|value|String|바인드하려는 Model 속성 이름.|

### @SessionAttributes
---
@SessionAttributes는 model attribute를 session에 저장, 유지할 때 사용하는 어노테이션이다.<br>
@SessionAttributes는 클래스 레벨(type level)에서 선언할 수 있다.<br>
[ 관련 속성 ]

|이름|타입|설명|
|:----:|:----:|:----:|
|value|String[]|session에 저장하려는 model attribute의 이름|
|required|Class[]|session에 저장하려는 model attribute의 타입|

### @CommandMap(실행환경 3.2부터deprecated됨 @RequestParam으로 대체)
---
실행환경 3.0부터 추가되었으며 Controller에서 Map형태로 웹요청 값을 받았을 때 다른 Map형태의 argument와 구분해주기 위한 어노테이션이다.<br>
@CommandMap은 파라미터 레벨(type level)에서 선언할 수 있다.<br>
사용 방법은 다음과 같다.

```java
@RequestMapping("/test.do")
public void test(@CommandMap Map<String, String> commandMap, HttpServletRequest request){
  //생략
}
```
CommandMap을 이용하기 위해서는 반드시 EgovRequestMappingHandlerAdapter와 함께 AnnotationCommandMapArgumentResolver를 등록해주어야 한다.

```xml
<bean class="egovframework.rte.ptl.mvc.bind.annotation.EgovRequestMappingHandlerAdapter">
  <property name="customArgumentResolvers">
    <list>
    <bean class="egovframework.rte.ptl.mvc.bind.AnnotationCommandMapArgumentResolver" />
    </list>
  </property>
</bean>
```

### @Controller 메소드 시그니쳐
---
기존의 계층형 Controller(SimpleFormController, MultiAction..)에 비해 유연한 메소드 파라미터, 리턴값을 갖는다.

### @Controller 메소드 시그니쳐 - 메소드 파라미터
- Servlet API - ServletRequest, HttpServletRequest, HttpServletResponse, HttpSession 같은 요청,응답,세션관련 Servlet API.
- org.springframework.web.context.request.WebRequest, org.springframework.web.context.request.NativeWebRequest
- java.util.Locale
- java.io.InputStream / java.io.Reader
- java.io.OutputStream / java.io.Writer
- @RequestParam - HTTP Request의 파라미터와 메소드의 argument를 바인딩하기 위해 사용하는 어노테이션.
- java.util.Map / org.springframework.ui.Model / org.springframework.ui.ModelMap > 뷰에 전달할 모델데이터.
- Command/form 객체 - HTTP Request로 전달된 parameter를 바인딩한 커맨드 객체, @ModelAttribute을 사용하면 alias를 줄수 있다.
- org.springframework.validation.Errors / org.springframework.validation.BindingResult > 유효성 검사 후 결과 데이터를 저장한 객체.
- org.springframework.web.bind.support.SessionStatus > 세션폼 처리시에 해당 세션을 제거하기 위해 사용된다.

### @Controller 메소드 시그니쳐 - 메소드 리턴 타입
---
- ModelAndView<br>
커맨드 객체, @ModelAttribute 적용된 메소드의 리턴 데이터가 담긴 Model 객체와 View 정보가 담겨 있다.
- Model(또는 ModelMap)<br>
커맨드 객체, @ModelAttribute 적용된 메소드의 리턴 데이터가 Model 객체에 담겨 있다.<br>
View 이름은 RequestToViewNameTranslator가 URL을 이용하여 결정한다.
- Map<br>
커맨드 객체, @ModelAttribute 적용된 메소드의 리턴 데이터가 Map 객체에 담겨 있으며, View 이름은 역시 RequestToViewNameTranslator가 결정한다.
- String<br>
리턴하는 String 값이 곧 View 이름이 된다. 커맨드 객체, @ModelAttribute 적용된 메소드의 리턴 데이터가Model(또는 ModelMap)에 담겨 있다.<br>
리턴할 Model(또는 ModelMap)객체가 해당 메소드의 argument에 선언되어 있어야 한다.
- void<br>
메소드가 ServletResponse / HttpServletResponse등을 사용하여 직접 응답을 처리하는 경우이다.<br>
View 이름은 RequestToViewNameTranslator가 결정한다.

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```