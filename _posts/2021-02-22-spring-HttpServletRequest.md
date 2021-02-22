---
title:  "[Spring] HttpServletRequest"
excerpt: "HttpServletRequest 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, HttpServletRequest]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-22
last_modified_at: 2021-02-22
---

### HttpServletRequest란?
---
javax.servlet.http에 속한 인터페이스로 javax.servlet에 있는 ServletRequest을 상속받습니다.<br>
클라이언트로부터 서버로 요청이 들어오면 서버에서는 HttpServletRequest를 생성하며, 요청정보에 있는 패스로 매핑된 서블릿에게 전달합니다.<br>
서블릿 컨테이너는 이렇게 전달받은 내용들을 파라미터(URL정보, 쿠키, 헤더, GET/POST 등으로 전송한 값)로 HttpServletRequest객체를 만들어 서블릿의 서비스 메서드(doGet, doPost 등)에 인자로 전달합니다.

### 메소드
---

|타입|메소드명|설명|원문|
|:----:|:----:|:----|:----|
|boolean|authenticate(HttpServletResponse response)|ServletContext에서 명시된 로그인 메커니즘을 이용하여 사용자 인증 여부 확인합니다.<br>인증 여부에 따라 true, false 반환합니다.|Use the container login mechanism configured for the ServletContext to authenticate the user making this request.|
|java.lang.String|getAuthType()|서블릿을 보호하는데 사용중인 authentication scheme의 이름을 반환합니다.<br>모든 서블릿 컨테이너는 basic, from 그리고 client certificate authentication을 지원하며 추가적으로 digest authentication이 지원될 수 있습니다.<br>아무 authentication이 지원되지 않으면 null 을 반환합니다.|Returns the name of the authentication scheme used to protect the servlet.|
|java.lang.String|getContextPath()|ServletContext.getContextPath()값을 반환합니다.|Returns the portion of the request URI that indicates the context of the request.|
|Cookie[]|getCookies()|클라이언트가 보낸 요청에 존재하는 모든 쿠키를 배열에 담아 가져옵니다.<br>쿠키가 존재하지 않을 경우 null 을 반환합니다.|Returns an array containing all of the Cookie objects the client sent with this request.|
|long|getDateHeader(java.lang.String name)|Date를 표현하는 long값을 반환합니다.<br>If-Modified-Since와 같이 date 정보를 갖고있는 헤더가 있을 때 사용할 수 있습니다.<br>날짜에 대한 값을 가진 헤더가 존재하지 않는다면 -1을 반환합니다.<br>IllegalArgumentException: header 값이 date로 전환될 수 없는 예외|Returns the value of the specified request header as a long value that represents a Date object.|
|java.lang.String|getHeader(java.lang.String name)|name에 해당되는 헤더의 값을 반환합니다.<br>name에 해당되는 헤더가 존재하지 않으면 null을 반환합니다.<br>header name은 case-insensitive(대소문자 구분).<br>name에 해당하는 헤더의 값이 많다면 가장 첫번째 값을 반환합니다.<br>모든 값을 받고싶다면 getHeaders()를 사용|Returns the value of the specified request header as a String.|
|java.util.Enumeration< java.lang.String >|getHeaderNames()|요청에 들어간 모든 헤더 name에 대한 Enumeration을 반환합니다.<br>일부 서블릿 컨테이너는 서블릿에서 header 정보에 접근하지 못하게 하고 이 경우 null을 반환합니다.|Returns an enumeration of all the header names this request contains.|
|java.util.Enumeration< java.lang.String >|getHeaders(java.lang.String name)|name에 해당되는 모든 헤더 값에 대한 Enumeration을 반환합니다.<br>대소문자를 구분하며, 해당하는 헤더 값이 없다면 empty Enumeration이 반환됩니다.|Returns all the values of the specified request header as an Enumeration of String objects.|
|int|getIntHeader(java.lang.String name)|name에 해당되는 헤더의 값을 int형으로 반환합니다.<br>name에 해당되는 헤더가 존재하지 않으면 -1을 반환합니다.<br>NumberFormatException: int형으로 변환할 수 없는 예외|Returns the value of the specified request header as an int.|
|java.lang.String|getMethod()|HTTP Method 이름을 반환합니다.<br>ex) GET, POST, PUT|Returns the name of the HTTP method with which this request was made, for example, GET, POST, or PUT.|
|Part|getPart(java.lang.String name)|name에 해당하는 part를 가져옵니다.|Gets the Part with the given name.|
|java.util.Collection< Part >|getParts()|multipart/form-data 요청에 모든 part들 가져옵니다.|Gets all the Part components of this request, provided that it is of type multipart/form-data.|
|java.lang.String|getPathInfo()|pathInfo를 반환합니다. query string은 제외입니다.<br>extra path information 정보가 없다면 null 값을 반환합니다.|Returns any extra path information associated with the URL the client sent when it made this request.|
|java.lang.String|getPathTranslated()|path info을 real path로 변환한 값을 반환합니다.<br>extra path information 정보가 없다면 null 값을 반환합니다.|Returns any extra path information after the servlet name but before the query string, and translates it to a real path.|
|java.lang.String|getQueryString()|요청의 query string을 반환합니다.<br>query string이 없다면 null을 반환합니다.|Returns the query string that is contained in the request URL after the path.|
|java.lang.String|getRemoteUser인증된 사용자에게 로그인에 대한 정보를 반환합니다.<br>인증되지 않았다면 null을 반환합니다.|Returns the login of the user making this request, if the user has been authenticated, or null if the user has not been authenticated.|
|java.lang.String|getRequestedSessionId()|클라이언트의 sessionId를 반환합니다.<br>현재 유효한 sessionId 와 같지 않아야 합니다.<br>클라이언트가 session ID가 없다면 null을 반환합니다.|Returns the session ID specified by the client.|
|java.lang.String|getRequestURI()|URI를 반환합니다.<br>ex) GET http://foo.bar/a,html HTTP/1.0 -> /a.html|Returns the part of this request's URL from the protocol name up to the query string in the first line of the HTTP request.|
|java.lang.StringBuffer|getRequestURL()|URL을 반환합니다.<br>forward된 요청일 경우, server path가 아닌, RequestDispatcher에서 반영된 경로를 반환합니다.|Reconstructs the URL the client used to make the request.|
|java.lang.String|getServletPath()|servlet path를 반환합니다.|Returns the part of this request's URL that calls the servlet.|
|HttpSession|getSession()|현재 세션을 가져오고 존재하지 않으면 생성하여 반환합니다.|Returns the current session associated with this request, or if the request does not have a session, creates one.|
|HttpSession|getSession(boolean create)|요청과 관련된 세션을 반환합니다..<br>세션이 없고 create = true일 경우 새로운 세션을 반환합니다.|Returns the current HttpSession associated with this request or, if there is no current session and create is true, returns a new session.|
|java.security.Principal|getUserPrincipal()|현재 인증된 사용자에 대한 Principal 객체를 반환합니다.|Returns a java.security.Principal object containing the name of the current authenticated user.|
|boolean|isRequestedSessionIdFromCookie()|Session이 HTTP cookie로 전달되었으면 true 아닐경우 false 반환합니다.|Checks whether the requested session ID came in as a cookie.|
|boolean|isRequestedSessionIdFromUrl()||Deprecated. As of Version 2.1 of the Java Servlet API, use isRequestedSessionIdFromURL() instead.|
|boolean|isRequestedSessionIdFromURL()|Session이 URL의 부분으로 전달되었다면 true 아닐경우 false 반환합니다.|Checks whether the requested session ID came in as part of the request URL.|
|boolean|isRequestedSessionIdValid()|유효한 session 일경우 true 아닐경우 false 반환합니다.|Checks whether the requested session ID is still valid.|
|boolean|isUserInRole(java.lang.String role)|인증된 사용자에 대해 특정 logical role에 대한 권한이 있는가의 여부를 반환합니다.|Returns a boolean indicating whether the authenticated user is included in the specified logical "role".|
|void|login(java.lang.String username, java.lang.String password)|username과 password로 login을 진행합니다.<br>ServletContext에서 정의한 login 메커니증에 따라 성공적으로 로그인되었다면 getUserPrincipal, getRemoteUser, getAuthType의 값이 만들어져 반환됩니다.|Validate the provided username and password in the password validation realm used by the web container login mechanism configured for the ServletContext.|
|void|logout()|로그아웃 이후 getUserPrincipal, getRemoteUser, getAuthType 함수는 null을 반환하게 됩니다.|Establish null as the value returned when getUserPrincipal, getRemoteUser, and getAuthType is called on the request.|

