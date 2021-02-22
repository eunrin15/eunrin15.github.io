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
클라이언트의 요청 정보를 서블릿으로 넘겨주기 위한 객체(요청에 대한 정보를 가진 객체)입니다.<br>
서블릿 컨테이너가 ServletRequest를 생성하고 객체를 서블릿의 service 메서드의 파라미터로 전달합니다.<br>
ServletRequest에는 parameter의 이름과 값, 속성, inputStream과 같은 데이터를 제공합니다.<br>
HTTP Protocol에서 존재하는 추가적인 데이터는 ServletRequest를 상속한 [**HttpServletRequest**](https://eunrin15.github.io/spring/spring-HttpServletRequest)에서 제공합니다.<br>
HTTP가 사용되지 않을 적, Web Conainer는 클라이언트로 부터 온 요청을 ServletRequest를 통해 전달했습니다.<br>
HTTP 등장이후 Web Conainer는 HttpServletRequest를 사용하였습니다.

### 메소드
---

|타입|메소드명|설명|원문|
|:----:|:----:|:----|:----|
|AsyncContext|getAsyncContext()||Gets the AsyncContext that was created or reinitialized by the most recent invocation of startAsync() or startAsync(ServletRequest,ServletResponse) on this request.|
|java.lang.Object|getAttribute(java.lang.String name)|Request가 갖는 속성 name에 대한 값을 반환합니다.|Returns the value of the named attribute as an Object, or null if no attribute of the given name exists.|
|java.util.Enumeration< java.lang.String >|getAttributeNames()|Request가 갖는 속성들의 name들에 대한 Enumeration 객체를 반환합니다.|Returns an Enumeration containing the names of the attributes available to this request.|
|java.lang.String|getCharacterEncoding()|Request Body에 사용된 인코딩 정보를 얻습니다.|Returns the name of the character encoding used in the body of this request.|
|int|getContentLength()|Request Body의 byte 길이를 얻습니다.<br>inputStream으로 body를 읽어올텐데 stream의 길이를 찾는데 사용됩니다.<br>길이를 모르면 -1 을 반환하고, Integer.MAX_VALUE 보다 길다면 Integer.MAX_VALUE 를 반환합니다.|Returns the length, in bytes, of the request body and made available by the input stream, or -1 if the length is not known.|
|java.lang.String|getContentType()|Request Body의 MIME type을 반환합니다.<br>모를경우 null을 반환합니다.|Returns the MIME type of the body of the request, or null if the type is not known.|
|DispatcherType|getDispatcherType()||Gets the dispatcher type of this request.|
|ServletInputStream|getInputStream()|binary data로 Request Body 정보를 담은 ServletInputStream(inputstream)을 반환합니다.<br>body를 읽기위해 이 메서드 또는 getReader() 메서드도 이용가능합니다.<br>IllegalStateException: getReader() 메서드가 이미 호출되었을 때의 예외(Stream은 한번만 읽을 수 있습니다.)<br>IOException: inputstream에서 발생하는 예외|Retrieves the body of the request as binary data using a ServletInputStream.|
|java.lang.String|getLocalAddr()|Request를 수신한 IP 주소를 반환합니다.|Returns the Internet Protocol (IP) address of the interface on which the request was received.|
|java.util.Locale|getLocale()|클라이언트에게 제공할 기본 Local을 반환합니다.<br>Accept-Language Header 정보를 통해 설정됩니다.<br>Accept-Language Header 정보가 존재하지 않다면 서버의 기본 local로 지정됩니다.|Returns the preferred Locale that the client will accept content in, based on the Accept-Language header.|
|java.util.Enumeration< java.util.Locale >|getLocales()|클라이언트에게 제공할 기본 Local들에 대한 Enumeration을 반환합니다.|Returns an Enumeration of Locale objects indicating, in decreasing order starting with the preferred locale, the locales that are acceptable to the client based on the Accept-Language header.|
|java.lang.String|getLocalName()|Request를 수신한 Host 이름을 반환합니다.|Returns the host name of the Internet Protocol (IP) interface on which the request was received.|
|int|getLocalPort()|Request를 수신한 포트 번호를 반환합니다.|Returns the Internet Protocol (IP) port number of the interface on which the request was received.|
|java.lang.String|getParameter(java.lang.String name)|name에 일치하는 request parameter를 반환합니다.<br>name에 일치하는 parameter가 존재하지 않을 시 null 반환합니다.<br>name에 일치하는 parameter가 두개 이상일 경우 첫번째 값을 반환합니다.<br>name에 일치하는 모든 parameter를 가져오고 싶을 땐 getParameterValues()를 사용해야 합니다.<br>HTTP Post와 같이 parameter가 request body와 함께 전송된다면 getInputStream() 과 getReader() 메서드를 호출한 경우 이 메서드의 실행을 방해할 수 있습니다.|Returns the value of a request parameter as a String, or null if the parameter does not exist.|
|java.util.Map< java.lang.String,java.lang.String[] >|getParameterMap()|Request에 대한 parameter 정보를 java.util.Map 형태로 반환합니다.<br>HttpServlet에서는 query string과 posted form data도 함께 포함되어 반환됩니다.|Returns a java.util.Map of the parameters of this request.|
|java.util.Enumeration< java.lang.String >|getParameterNames()|Request가 갖는 parameter name들에 대한 Enumeration 객체를 반환합니다.|Returns an Enumeration of String objects containing the names of the parameters contained in this request.|
|java.lang.String[]|getParameterValues(java.lang.String name)|name에 일치하는 모든 parameter를 배열로 반환합니다.name에 일치하는 parameter가 존재하지 않을 시 null 반환합니다.|Returns an array of String objects containing all of the values the given request parameter has, or null if the parameter does not exist.|
|java.lang.String|getProtocol()|Request에 사용된 프로토콜 정보를 반환합니다.<br>form: protocol/majorVersion.minorVersion|Returns the name and version of the protocol the request uses in the form protocol/majorVersion.minorVersion, for example, HTTP/1.1.|
|java.io.BufferedReader|getReader()|Character Data로 Request Body 정보를 담은 BufferedReader를 반환합니다.<br>characterEncoding 방법에 맞게 해석하여 반환해줍니다<br>UnsupportedEncodingException: 지원되지 않는 인코딩이거나 해석되지 못할 때<br>IllegalStateException: getInputStream() 메서드가 이미 호출되었을 때의 예외(Stream은 한번만 읽을 수 있습니다.)<br>IOException: inputstream에서 발생하는 예외|Retrieves the body of the request as character data using a BufferedReader.|
|java.lang.String|getRealPath(java.lang.String path)||Deprecated. As of Version 2.1 of the Java Servlet API, use ServletContext#getRealPath instead.|
|java.lang.String|getRemoteAddr()|요청 클라이언트의 IP 주소를 반환합니다.|Returns the Internet Protocol (IP) address of the client or last proxy that sent the request.|
|java.lang.String|getRemoteHost()|요청 클라이언트의 호스트 이름을 반환합니다.|Returns the fully qualified name of the client or the last proxy that sent the request.|
|int|getRemotePort()|Request를 보내 클라이언트의 IP 포트 번호, 혹은 가장 마지막 프록시의 포트 번호를 반환합니다.|Returns the Internet Protocol (IP) source port of the client or last proxy that sent the request.|
|RequestDispatcher|getRequestDispatcher(java.lang.String path)|해당 path의 RequestDispatcher를 반환합니다.<br>서블릿 컨테이너가 RequestDispatcher를 반환할 수 없다면 null을 반환합니다.|Returns a RequestDispatcher object that acts as a wrapper for the resource located at the given path.|
|java.lang.String|getScheme()|Request를 만들기 위해 사용된 방법의 이름을 반환합니다.<br>ex) http, https, ftp|Returns the name of the scheme used to make this request, for example, http, https, or ftp.|
|java.lang.String|getServerName()|서버의 이름을 반환합니다.|Returns the host name of the server to which the request was sent.|
|int|getServerPort()|서버에서 사용중인 포트 번호를 반환합니다.|Returns the port number to which the request was sent.|
|ServletContext|getServletContext()|가장 최근 해당 ServletRequest를 dispatch한 ServletContext를 반환합니다.|Gets the servlet context to which this ServletRequest was last dispatched.|
|booleanisAsyncStarted()||Checks if this request has been put into asynchronous mode.|
|boolean|isAsyncSupported()||Checks if this request supports asynchronous operation.|
|boolean|isSecure()|안전한 채널을 통해 전송되었는지 여부를 반환합니다.|Returns a boolean indicating whether this request was made using a secure channel, such as HTTPS.|
|void|removeAttribute(java.lang.String name)|주어진 이름의 속성을 제거합니다.|Removes an attribute from this request.|
|void|setAttribute(java.lang.String name, java.lang.Object o)|주어진 이름의 속성을 설정합니다.|Stores an attribute in this request.|
|void|setCharacterEncoding(java.lang.String env)|Request Body에 대해 인코딩한 정보를 정의합니다.<br>UnsupportedEncodingException: 이미 인코딩 정보가 지정되어 있을 때 발생하는 예외|Overrides the name of the character encoding used in the body of this request.|
|AsyncContext|startAsync()|해당 request를 비동기모드로 설정합니다.|Puts this request into asynchronous mode, and initializes its AsyncContext with the original (unwrapped) ServletRequest and ServletResponse objects.|
|AsyncContext|startAsync(ServletRequest servletRequest, ServletResponse servletResponse)||Puts this request into asynchronous mode, and initializes its AsyncContext with the given request and response objects.|

### 관련 포스팅
---
[**HttpServletRequset**](https://eunrin15.github.io/spring/spring-HttpServletRequest)

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```