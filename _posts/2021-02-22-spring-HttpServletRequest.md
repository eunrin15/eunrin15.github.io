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
서블릿 컨테이너는 이렇게 전달받은 내용들을 파라미터(URL정보, 쿠키, 헤더, GET/POST 등으로 전송한 값)로 HttpServletRequest객체를 만들어 서블릿의 서비스 메서드(doGet, doPost 등)에 인수로 전달합니다.

### Method Summary
---
|타입|메소드명|설명|
|:----:|:----:|:----|
|boolean|authenticate(HttpServletResponse response)|Use the container login mechanism configured for the ServletContext to authenticate the user making this request.|
|java.lang.String|getAuthType()|Returns the name of the authentication scheme used to protect the servlet.|
|java.lang.String|getContextPath()|Returns the portion of the request URI that indicates the context of the request.|
|Cookie[]|getCookies()|Returns an array containing all of the Cookie objects the client sent with this request.|
|long|getDateHeader(java.lang.String name)|Returns the value of the specified request header as a long value that represents a Date object.|
|java.lang.String|getHeader(java.lang.String name)|Returns the value of the specified request header as a String.|
|java.util.Enumeration<java.lang.String>|getHeaderNames()|Returns an enumeration of all the header names this request contains.|
|java.util.Enumeration<java.lang.String>|getHeaders(java.lang.String name)|Returns all the values of the specified request header as an Enumeration of String objects.|
|int|getIntHeader(java.lang.String name)|Returns the value of the specified request header as an int.|
|java.lang.String|getMethod()|Returns the name of the HTTP method with which this request was made, for example, GET, POST, or PUT.|
|Part|getPart(java.lang.String name)|Gets the Part with the given name.|
|java.util.Collection<Part>|getParts()|Gets all the Part components of this request, provided that it is of type multipart/form-data.|
|java.lang.String|getPathInfo()|Returns any extra path information associated with the URL the client sent when it made this request.|
|java.lang.String|getPathTranslated()|Returns any extra path information after the servlet name but before the query string, and translates it to a real path.|
|java.lang.String|getQueryString()|Returns the query string that is contained in the request URL after the path.|
|java.lang.String|getRemoteUser()|Returns the login of the user making this request, if the user has been authenticated, or null if the user has not been authenticated.|
|java.lang.String|getRequestedSessionId()|Returns the session ID specified by the client.|
|java.lang.String|getRequestURI()|Returns the part of this request's URL from the protocol name up to the query string in the first line of the HTTP request.|
|java.lang.StringBuffer|getRequestURL()|Reconstructs the URL the client used to make the request.|
|java.lang.String|getServletPath()|Returns the part of this request's URL that calls the servlet.|
|HttpSession|getSession()|Returns the current session associated with this request, or if the request does not have a session, creates one.|
|HttpSession|getSession(boolean create)|Returns the current HttpSession associated with this request or, if there is no current session and create is true, returns a new session.|
|java.security.Principal|getUserPrincipal()|Returns a java.security.Principal object containing the name of the current authenticated user.|
|boolean|isRequestedSessionIdFromCookie()|Checks whether the requested session ID came in as a cookie.|
|boolean|isRequestedSessionIdFromUrl()|Deprecated. As of Version 2.1 of the Java Servlet API, use isRequestedSessionIdFromURL() instead.|
|boolean|isRequestedSessionIdFromURL()|Checks whether the requested session ID came in as part of the request URL.|
|boolean|isRequestedSessionIdValid()|Checks whether the requested session ID is still valid.|
|boolean|isUserInRole(java.lang.String role)|Returns a boolean indicating whether the authenticated user is included in the specified logical "role".|
|void|login(java.lang.String username, java.lang.String password)|Validate the provided username and password in the password validation realm used by the web container login mechanism configured for the ServletContext.|
|voidlogout()|Establish null as the value returned when getUserPrincipal, getRemoteUser, and getAuthType is called on the request.|