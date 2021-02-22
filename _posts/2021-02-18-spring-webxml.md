---
title:  "[Spring] web.xml 파일 구조"
excerpt: "web.xml 파일 구조 정리"

categories:
  - Spring
tags:
  - [Java, web.xml, Spring]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-18
last_modified_at: 2021-02-19
---

### web.xml이란?
---
애플리케이션의 클래스, 리소스, 구성을 기술하고, 웹 서버가 이를 사용해서 웹 요청을 처리하는 방법을 기술합니다.<br>
웹 서버가 애플리케이션에 대한 요청을 수신하면 web.xml을 사용해서 해당 요청을 처리해야 하는 코드로 요청의 URL을 매핑합니다.

### web-app 태그
---
web.xml 파일의 루트 엘리먼트(root element)입니다.<br>
모든 웹 애플리케이션 설정은 web-app 태그 내부에 위치하여야 합니다. web.xml 생성시 자동으로 등록되어 있습니다.

### display-name 태그
---
기본적으로 프로젝트명으로 설정되며 수정이 가능합니다.

```xml
<display-name>ProjectName</display-name>
```

### description 태그
---
어떤 프로젝트를 위한 web.xml 파일인지 설명을 적는 부분입니다.

```xml
<description>Project description</description>
```

### servlet 태그
---
web.xml은 URL 경로와 해당 경로의 요청을 처리하는 서블릿 사이의 매핑을 정의합니다.<br>
웹 서버는 이 구성을 사용하여 특정한 요청을 처리할 서블릿을 식별하고 요청 메서드에 해당하는 클래스 메서드를 호출합니다.<br>
URL을 서블릿에 매핑하려면 servlet 요소로 서블릿을 선언한 후 servlet-mapping 요소로 URL 경로에서 서블릿 선언으로 이어지는 매핑을 정의합니다.<br>
servlet 요소에는 파일의 다른 요소에서 서블릿을 참조하는 데 사용할 이름, 서블릿에 사용할 클래스, 초기화 매개변수가 포함됩니다.<br>
여러 초기화 매개변수에 동일한 클래스를 사용해서 여러 서블릿을 선언할 수 있습니다. 각 서블릿의 이름은 web.xml에서 고유해야 합니다.

```xml
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring/dispatcher-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup> // servlet 최초 요청시 우선순위 지정옵션
</servlet>

<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

위의 예제는 DispatcherServlet 클래스를 초기화하여 spring의 servlet context를 생성합니다.<br>
초기화 param으로 bean 메타 설정 파일의 위치를 넘겨 줍니다.<br>
servlet-mapping은 url pattern으로 지정된 값으로 웹 요청이 들어왔을 때 servlet-name에 지정되어 있는 이름의 servlet을 호출하겠다는 의미입니다.<br>
spring에서는 DispatcherServlet이 모든 요청을 받고, 요청의 URL과 맵핑하는 Controller에 위임합니다.<br>
예를 들어 Controller class에 @RequestMapping("/list") annotation으로 지정되어 잇는 method가 존재하면 http://localhost:8080/list.do 요청 시 DispatcherServlet이 해당하는 URL과 mapping되는 정보를 찾은 후 연결되는 method에 요청을 위임합니다.<br>
즉, *.do에 해당하는 요청이 있을경우 /WEB-INF/spring/dispatcher-servlet.xml가 호출이 됩니다.

### ContextLoaderListener
---
Spring MVC는 web.xml - dispatcher.xml 로부터 1개 이상의 스프링 설정 정보를 읽어들일수 있습니다.<br>
이때 한개만으로 충분한 경우 dispatcher 에 지정하면 되지만 2개 이상이면 너무 복잡해지므로 contextConfigLocation 초기화 파라미터에 설정파일 목록을 지정하면 됩니다.

```xml
<listener>
	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
        /WEB-INF/spring/application-context.xml
        /WEB-INF/spring/application-context-security.xml
    </param-value>
</context-param>
```

### JSP
---
앱은 JSP(Java Server Pages)를 사용해서 웹 페이지를 구현할 수 있습니다.<br>
JSP는 자바 코드와 혼합된 정적 콘텐츠(예: HTML)를 사용하여 정의된 서블릿입니다.<br>
App Engine은 JSP 자동 컴파일 및 URL 매핑을 지원합니다.<br>
애플리케이션의 WAR(WEB-INF/)에서 파일 이름이 .jsp로 끝나는 JSP 파일은 서블릿 클래스로 자동으로 컴파일되며 WAR 루트에서 JSP 파일 경로에 해당하는 URL 경로에 매핑됩니다.<br>
예를 들어 WAR의 register/ 하위 디렉토리에 이름이 start.jsp인 JSP 파일이 앱에 포함된 경우 App Engine이 이를 컴파일하여 URL 경로 /register/start.jsp로 매핑합니다.<br>
JSP가 URL에 매핑되는 방식을 더 세밀하게 제어하려면 배포 설명자의 servlet 요소에 매핑을 선언하여 명시적으로 지정할 수 있습니다.<br>
servlet-class 요소 대신 WAR 루트의 JSP 파일에 대한 경로로 jsp-file 요소를 지정합니다.<br>
JSP의 servlet 요소는 초기화 매개변수를 포함할 수 있습니다.<br>
JSP가 애플리케이션의 루트 디렉터리에 있는 경우 jsp-file은 슬래시(/)로 시작해야 합니다.

```xml
<servlet>
    <servlet-name>register</servlet-name>
    <jsp-file>/register/start.jsp</jsp-file>
</servlet>

<servlet-mapping>
    <servlet-name>register</servlet-name>
    <url-pattern>/register/*</url-pattern>
</servlet-mapping>
```

### taglib 태그
---
JSP 태그 라이브러리를 설치할 수 있습니다.<br>
태그 라이브러리에는 JSP TLD(태그 라이브러리 설명자) 파일의 경로(taglib-location) 및 JSP에서 로드할 라이브러리를 선택하는 데 사용하는 URI(taglib-uri)가 포함됩니다.<br>
App Engine은 JSTL(자바 서버 페이지 표준 태그 라이브러리)은 App Engine에서 제공하므로 직접 설치할 필요가 없습니다.

```xml
<taglib>
    <taglib-uri>/escape</taglib-uri>
    <taglib-location>/WEB-INF/escape-tags.tld</taglib-location>
</taglib>
```

### welcome-file-list 태그
---
웹 요청 시 시작파일을 지정하는 부분입니다.

```xml
<welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>index.html</welcome-file>
</welcome-file-list>
```

### session-config 태그
---
session 시간 설정을 하는 부분입니다. (단위 > 분)

```xml
<session-config>
    <session-timeout>60</session-timeout>
</session-config>
```

### error-page 태그
---
오류가 발생할 때 어떤 서버가 사용자에게 이를 전송할지를 설정할 수 있습니다.<br>
HTTP 오류 코드 값이 있는 error-code 요소(예: 500) 또는 예상되는 예외의 클래스 이름이 있는 exception-type 요소(예: java.io.IOException)를 포함합니다.<br>
또한 오류가 발생할 때 표시할 리소스의 URL 경로를 갖는 location 요소가 포함됩니다.<br>
즉, 에러코드에 따른 에러 페이지를 골라서 보여주도록 설정하는 부분입니다.

```xml
<error-page>
    <error-code>500</error-code>
    <location>/errors/servererror.jsp</location>
</error-page>
```

### filter 태그
---
필터는 웹 어플리케이션 전반에 걸쳐 특정 URL이나 파일 요청 시 먼저 로딩되어 사전에 처리할 작업을 수행합니다.<br>
예를들어 UTF-8로 텍스트를 인코딩하거나 스프링 시큐리티관련 내용을 작성할때도 필터를 사용합니다.<Br>
필터를 구성하는 방법은 서블릿과 비슷합니다.<br>
filter 요소로 필터를 선언한 후 filter-mapping 요소로 URL 패턴에 매핑하면 됩니다.<bR>
또한 다른 서블릿에 필터를 직접 매핑할 수 있습니다.<br>
filter 요소에는 filter-name, filter-class, 선택사항 init-param 요소가 포함됩니다.<br>
filter-mapping 요소에는 선언된 필터의 이름과 일치하는 filter-name이 포함되고,<br>
URL에 필터를 적용하는 url-pattern 요소 또는 선언된 서블릿의 이름과 일치하며 해당 서블릿이 호출될 때마다 필터를 적용하는 servlet-name 요소 중 하나가 포함됩니다.

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <url-pattern>*.do</url-pattern>
</filter-mapping>
```

### login-config 태그
---
- Impersonator<br>
자신의 정체를 속여서 요청을 보내는 기법
- Upgrader<br>
자신의 등급을 속이는 기법
- Eavesdropper<br>
요청을 중간에 훔쳐보는 기법

- 서블릿보안 4요소
1. 인증(Authentication)
2. 인가(Authorization)
3. 비밀보장(Confidentiality)
4. 데이터무결성(Data Integrity)

- 인증
인증은 4가지 방식을 지원하는데 BASIC, DIGEST, CLIENT-CERT, FORM 방식이 있다.<br>
1. BASIC<br>
HTTP 표준
2. DIGEST<br>
HTTP, J2EE 컨테이너에서 옵션
3. CLIENT-CERT<br>
사용자가 인증서를 갖고있어야함
4. FORM<br>
사용자정의 로그인화면

j_username, j_password, j_security_check 와 같이 정해진 명칭으로 데이터를 전송해야합니다.<br>
스프링 시큐리티가 이것을 기반으로 합니다.<br>
해당 인증 방식은 web.xml에 기술하게되는데 login-config 태그 내부에 작성합니다.

```xml
<login-config>
    <auth-method>BASIC</auth-method>
</login-config>

<login-config>
    <auth-method>FORM</auth-method>
    <form-login-config>
        <form-login-page>/login.html</form-login-page>
        <form-error-page>/error.html</form-error-page>
    </form-login-config>
</login-config>
```

- 인가<br>
인가에 관련된것은 web.xml 에 기술하게 되는데 security-constraint 태그 내부에 기술하게 됩니다.

```xml
<security-constraint>
    <web-resource-collection>
        <web-resource-name></web-resource-name>
        <url-pattern>*</url-pattern>
        <http-method>GET</http-method>
    </web-resource-collection>

    <auth-constraint>
        <role-name></role-name>
    </auth-constraint>
</security-constraint>
```

### multipart-config 태그
---
파일의 업로드의 옵션을 설정하여 제한할 수 있습니다.

```xml
<multipart-config>
    <location>c:/directory</location>
    <max-file-size>209715200</max-file-size>
    <max-request-size>209715200</max-request-size>
    <file-size-threshold>0</file-size-threshold>
</multipart-config>
```

- location<br>
파일 업로드 시 임시로 저장하는 절대 경로
- maxFileSize<br>
파일당 최대 파일 크기
- maxRequestSize<br>
파일 한 개의 용량이 아니라 multipart/form-data 요청당 최대 파일 크기이다 (여러 파일 업로드 시 총 크기로 보면 된다) * 디폴트 값: 제한없음
- fileSizeThreshold<br>
업로드하는 파일이 임시로 파일로 저장되지 않고 메모리에서 바로 스트림으로 전달되는 크기의 한계를 나타낸다

### load-on-startup
---
서블릿이 로딩될 때 로딩 순서를 결정하는 값입니다.<br>
톰캣이 구동되고 서블릿이 로딩되기 전 해당 서블릿에 요청이 들어오면 서블릿이 구동되기 전까지 기다려야 합니다.<br>
이 중 우선순위가 높은 서블릿부터 구동할 때 쓰이는 값입니다.

```xml
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring/dispatcher-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

### 관련 포스팅
---
[**Dispatcher-Servlet**](https://eunrin15.github.io/spring/spring-dispatcher-servlet)