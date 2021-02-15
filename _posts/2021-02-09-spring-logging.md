---
title:  "[Spring] Logging"
excerpt: "MVC 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, Logging]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

### 서비스 개요
---
- Logging은 시스템의 개발이나 운용 시 발생할 수 있는 어플리케이션 내부정보에 대해서, 시스템의 외부 저장소에 기록하여, 시스템의 상황을 쉽게 파악할 수 있게 지원하는 서비스
- 표준프레임워크 3.0부터 SLF4J가 적용이 되었으며 Log4j를 함께 사용하였다.

### 주요 기능
---
- 로깅 환경 설정 지원<br>
서브 시스템 별 상세한 로그 정책 부여<br>
다양한 형식(날짜 형식, 시간 형식 등)의 로그 메시지 형태 지정<br>
다양한 매체(File, DBMS, Message, Mail 등)에 대한 기록 기능 설정
- 로그 기록<br>
레벨(debug, info, warn, error 등)별로 로그를 기록

### Logging 서비스
---
- 시스템의 개발이나 운용시 발생할 수 있는 사항에 대해서, 시스템의 외부 저장소에 기록하여, 시스템의 상황을 쉽게 파악할 수 있음.
- 많은 개발자가 Log을 출력하기 위해 일반적으로 사용하는 방식은 System.out.println()임.<br>
하지만 이 방식은 간편한 반면에 다음과 같은 이유로 권장하지 않음.
1. 콘솔 로그를 출력 파일로 리다이렉트 할 지라도, 어플리케이션 서버가 재 시작할 때 파일이 overwrite될 수도 있음.
2. 개발/테스팅 시점에만 System.out.println()을 사용하고 운영으로 이관하기 전에 삭제하는 것은 좋은 방법이 아님.
3. System.out.println() 호출은 디스크 I/O동안 동기화(synchronized)처리가 되므로 시스템의 throughput을 떨어뜨림.
4. 기본적으로 stack trace 결과는 콘솔에 남는다. 하지만 시스템 운영 중 콘솔을 통해 Exception을 추적하는 것은 바람직하지 못함.

### Log4j 환경 설정하는 방법
---
1. 프로그래밍내에서 직접 설정하는 방법
2. 설정 파일을 사용하는 방법

|컴포넌트|설명|
|:----:|:----|
|Logger|로그의 주체 (로그 파일을 작성하는 클래스)<br>설정을 제외한 거의 모든 로깅 기능이 이를 통해 처리됨.<br>어플리케이션 별로 사용할 로거(로거명 기반)를 정의하고 이에 대해 로그레벨과 Appender를 지정할 수 있음.|
|Appender|로그를 출력하는 위치를 의미하며, Log4J API문서의 XXXAppender로 끝나는 클래스들의 이름을 보면, 출력위치를 어느정도 짐작할 수 있음.|
|Layout|Appender의 출력포맷<br>일자, 시간, 클래스명등 여러가지 정보를 선택하여 로그정보내용으로 지정할 수 있음.|

### 로그레벨 지정하기
---
log4j에서는 기본적으로 debug, info, warn, error, fatal의 다섯 가지 로그레벨이 있음<br>
(ERROR > WARN > INFO > DEBUG > TRACE)

|로그 레벨|설명|
|:----:|:----|
|Error|요청을 처리하는중 문제가 발생한 상태를 나타냄|
|Warn|처리 가능한 문제이지만, 향후 시스템 에러의 원인이 될 수 있는 경고성 메시지를 나타냄.|
|Info|로그인, 상태변경과 같은 정보성 메시지를 나타냄.|
|Debug|개발시 디버그 용도로 사용할 메시지를 나타냄.|
|Trace|디버그 레벨이 너무 광범위한 것을 해결하기 위해서 좀 더 상세한 상태를 나타냄|

### Appender
---
log4j 는 콘솔, 파일, DB, socket, message, mail 등 다양한 로그 출력 대상과 방법을 지원하는데, 이에 대해 log4j 의 Appender 로 정의할 수 있다.

|Appender|설명|
|:----:|:----|
|ConsoleAppender|콘솔화면으로 출력하기 위한 appender임<br>org.apache.log4j.ConsoleAppender : Console 화면으로 출력하기 위한 Appender|
|FileAppender|FileAppender는 로깅을 파일에 하고 싶을 때 사용함.|
|RollingFileAppender|FileAppender는 지정한 파일에 로그가 계속 남으므로 한 파일의 크기가 지나치게 커질 수 있으며, 계획적인 로그관리가 불가능해짐.<br>RollingFileAppender는 파일의 크기 또는 파일백업인덱스 등의 지정을 통해서 특정크기 이상 파일의 크기가 커지게 되면 기존파일을 백업파일로 바꾸고, 다시 처음부터 로깅을 시작함|
|JDBCAppender|DB에 로그를 출력하기 위한 Appender로 하위에 Driver, URL, User, Password, Sql과 같은 parameter를 정의할 수 있음.<br>다음은 log4j.xml 파일 내의 JDBCAppender에 대한 속성 정의 내용임.<br>EgovConnectionFactory 빈 설정이 필요함|