---
title:  "[Spring] Maven"
excerpt: "Maven 개념 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, Maven]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-09
last_modified_at: 2021-02-15
---

### Maven이란?
---
자바 프로젝트의 빌드(build)를 자동화 해주는 빌드 툴(build tool)이다.
즉, 자바 소스를 compile하고 package해서 deploy하는 일을 자동화 해주는 것 이다.

### 빌드란?
---
소스코드 파일을 컴퓨터에서 실행할 수 있는 독립 소프트웨어 가공물로 변환하는 과정 또는 그에 대한 결과물 이다.
소스코드(Java), 각각의 파일 및 자원 등(xml,jpg,jar,propertoes)을 JVM이나 톰캣같은 WAS가 인식할 수 있는 구조로 패키징하는 과정 및 결과물이라고 할 수 있다.

### 빌드 도구(build tool)란?
---
프로젝트 생성, 테스트 빌드, 배포 등의 작업을 위한 전용 프로그램이다.

### Maven이 참조하는 설정 파일
---
1. Settings.xml<br>
Maven build tool에 관련된 설정을 담당한다.<br>MAVEN_HOME/conf 디렉토리에 위치해 있다.<br>

2. pom.xml<br>
POM(Project Object Model)을 설정하는 부분으로 프로젝트 내 빌드 옵션을 설정하는 부분이다.<br>프로젝트 최상위 디렉토리에 위치해있다.<br>파일은 프로젝트에 1개이며 pom.xml만 보면 프로젝트의 모든 설정, 의존성 등을 알 수 있다.<br>다른 이름으로 지정할 수 도 있지만 가급적 pom.xml으로 사용하길 권장한다.

### pom.xml 파일 분석
---

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion> <!--POM model의 버전-->
	<parent> <!--프로젝트의 계층 정보-->
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<groupId>com.god</groupId> <!--프로젝트를 생성하는 조직의 고유 아이디를 결정한다. 일반적으로 도메인 이름을 거꾸로 적는다.-->

	<artifactId>bo</artifactId> <!--프로젝트 빌드시 파일 대표이름 이다. groupId 내에서 유일해야 한다.Maven을 이용하여 빌드시 다음과 같은 규칙으로 파일이 생성 된다.
									artifactid-version.packaging. 위 예의 경우 빌드할 경우 bo-0.0.1-SNAPSHOT.war 파일이 생성된다.-->

	<version>0.0.1-SNAPSHOT</version> <!--프로젝트의 현재 버전, 프로젝트 개발 중일 때는 SNAPSHOT을 접미사로 사용-->

	<packaging>war</packaging> <!--패키징 유형(jar, war, ear 등)-->

	<name>bo</name> <!--프로젝트, 프로젝트 이름-->

	<description>Demo project for Spring Boot</description> <!--프로젝트에 대한 간략한 설명-->

	<url>http://goddaehee.tistory.com</url> <!--프로젝트에 대한 참고 Reference 사이트-->

	<properties>
	<!-- 버전관리시 용이 하다. ex) 하당 자바 버전을 선언 하고 dependencies에서 다음과 같이 활용 가능 하다.
	<version>${java.version}</version> -->
		<java.version>1.8</java.version>
	</properties>

	<dependencies> <!--dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리 한다.-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build> <!--빌드에 사용할 플러그인 목록-->
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

### 구성 요소(프로젝트 정보)
---
- modelVersion<br>
POM model의 버전
- parent<br>
프로젝트의 계층 정보
- groupId<br>
프로젝트를 생성하는 조직의 고유 아이디를 결정한다. 일반적으로 도메인 이름을 거꾸로 적는다.
- artifactId<br>프로젝트 빌드시 파일 대표이름 이다.
- version<br>
프로젝트의 현재 버전, 프로젝트 개발 중일 때는 SNAPSHOT을 접미사로 사용.
- packaging<br>
패키징 유형(jar, war, ear 등)
- name<br>
프로젝트, 프로젝트 이름
- description<br>
프로젝트에 대한 간략한 설명
- url<br>
프로젝트에 대한 참고 Reference 사이트
- properties<br>
버전관리시 용이 하다.<br>
하당 자바 버전을 선언 하고 dependencies에서 다음과 같이 활용 가능 하다.
- <version>${java.version}</version><br>
dependencies : dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리 한다.
- build<br>
빌드에 사용할 플러그인 목록

### 구성 요소(의존성 라이브러리 정보)
---
의존성 라이브러리 정보를 적을 수 있다.<br>
최소한 groupId, artifactId, version 정보가 필요하다.<br>
스프링부트의 spring-boot-starter-*같은 경우에는 부모 pom.xml에서 이미 버전정보가 있어서 version은 따로 지정할 필요가 없다.<br>
특히 스프링부트는 해당 스프링버전에 잘 맞는 버전으로 이미 설정되어 있기 때문에 오버라이드해서 문제가 생기는 부분은 순전히 개발자 탓이다.<br>
그리고 A라는 라이브러리를 사용하는데 B,C,D가 의존성을 가진다면 A를 dependency에 추가하면 자동으로 필요한 B,C,D도 가져오는 기능이 있다.<br>
dependency에 scope의 경우 compile, runtime, provided, test등이 올 수 있는데 해당 라이브러리가 언제 필요한지, 언제 제외되는지를 나타내는 것으로 따로 검색해보면 알 수 있다.

### 구성 요소(의존성 라이브러리 정보)
---
- build tool<br>
maven의 핵심인 빌드와 관련된 정보를 설정할 수 있는 곳이다.
- build<br>
부분에서 설정할 수 있는 값들에 대해 설명하기 전에 "라이프 사이클(life-cycle"에 대해서 알 필요가 있다.
- 객체의 생명주기처럼 maven에는 라이프 사이클이 존재한다.
- 크게 default, clean, site 라이프 사이클로 나누고 세부적으로 페이즈(phase) 있다.<br>

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile21.uf.tistory.com%2Fimage%2F999B12465BBC992C202A89" width="70%" height="50%" /><br>

- 메이븐의 모든 기능은 플러그인(plugin)을 기반으로 동작한다.
- 플러그인에서 실행할 수 있는 각각의 작업을 골(goal)이라하고 하나의 페이즈는 하나의 골과 연결되며, 하나의 플러그인에는 여러 개의 골이 있을 수 있다.

### 라이프 사이클
---
- mvn process-resources<br>
resources:resources의 실행으로 resource 디렉토리에 있는 내용을 target/classes로 복사한다.

- mvn compile<br>
compiler:compile의 실행으로 src/java 밑의 모든 자바 소스를 컴파일해서 target/classes로 복사

- mvn process-testResources, mvn test-compile<br>
이것은 위의 두 개가 src/java였다면 test/java의 내용을 target/test-classes로 복사. (참고로 test만 mvn test 명령을 내리면 라이프사이클상 원본 소스로 컴파일된다.)

- mvn test<br>
surefire:test의 실행으로 target/test-classes에 있는 테스트케이스의 단위테스트를 진행한다.<br>
결과를 target/surefire-reports에 생성한다.

- mvn package<br>
target디렉토리 하위에 jar, war, ear등 패키지파일을 생성하고 이름은 build의 finalName의 값을 사용한다.<br>
지정되지 않았을 때는 아까 설명한 "artifactId-version.extention" 이름으로 생성

- mvn install<br>
로컬 저장소로 배포

- mvn deploy<br>
원격 저장소로 배포

- mvn clean<br>
빌드 과정에서 생긴 target 디렉토리 내용 삭제

- mvn site<br>
target/site에 문서 사이트 생성

- mvn site-deploy<br>
문서 사이트를 서버로 배포

위와 같은 진행 순서로 라이프 사이클이 진행된다.<br>
이제 <build>에서 설정할 수 있는 값을 확인해보자.<br>

- finalName
빌드 결과물(ex .jar) 이름 설정<br>

- resources<br>
리소스(각종 설정 파일)의 위치를 지정할 수 있다.

- resource<br>
없으면 기본으로 "src/main/resources"

- testResources<br>
테스트 리소스의 위치를 지정할 수 있다.

- testResource<br>
없으면 기본으로 "src/test/resources"

- Repositories<br>
빌드할 때 접근할 저장소의 위치를 지정할 수 있다.<br>
기본적으로 메이븐 중앙 저장소인 http://repo1.maven.org/maven2로 지정되어 있다.

- outputDirectory<br>
컴파일한 결과물 위치 값 지정, 기본 "target/classes"

- testOutputDirectory<br>
테스트 소스를 컴파일한 결과물 위치 값 지정, 기본 "target/test-classes"

- plugin<br>
어떠한 액션 하나를 담당하는 것으로 가장 중요하지만 들어가는 옵션은 제 각각이다.<br>
다행인 것은 플러그인 형식에 대한 것은 안내가 나와있으니 그것을 참고해서 작성하면 된다.

- plugin이 작성되어 있다고 무조건 실행되는 것은 아니다.<br>
명확한 것은 아니지만 따로 실행할 플러그인을 메이븐 명령어로 실행해야 하는 것으로 알고 있다.

- executions><br>
플러그인 goal과 관련된 실행에 대한 설정

- configuration<br>
플러그인에서 필요한 설정 값 지정

### apache CXF를 이용한 code generate 플러그인은 아래에서 소개되고 사용한다.
---
http://cxf.apache.org/docs/maven-cxf-codegen-plugin-wsdl-to-java.html<br>

```xml
<plugin>
    <groupId>org.apache.cxf</groupId>
    <artifactId>cxf-codegen-plugin</artifactId>
    <version>${cxf.version}</version>
    <executions>
        <execution>
            <id>generate-sources</id>
            <phase>generate-sources</phase>
            <configuration>
                <sourceRoot>${project.build.directory}/generated/cxf</sourceRoot>
                <wsdlOptions>
                    <wsdlOption>
                        <wsdl>${basedir}/src/main/resources/myService.wsdl</wsdl>
                    </wsdlOption>
                </wsdlOptions>
            </configuration>
            <goals>
                <goal>wsdl2java</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### 배포
---

```xml
<project>
  ...
  <distributionManagement>
    <site>
      <id>website</id>
      <url>scp://www.mycompany.com/www/docs/project/</url>
    </site>
  </distributionManagement>
  ...
</project>
```

사이트로 배포할 때 위와 같이 설정할 수도 있다.

### 번외. <Parent> pom.xml 상속
---
- Parent<br>
pom.xml은 상속을 받을 수 있다.

- 스프링부트의 경우 부모 pom.xml에 자주 사용하는 라이브러리들의 버전정보나 dependency들을 이미 가지고 있어서 참조하기 편리하다.

- 참고로 super pom.xml이라는 것이 있다.<br>
모든 pom.xml이 기본적으로 상속하고 있는 부모 설정파일로 이것 때문에 기본으로 생성된 pom.xml에 별 내용이 없어도 잘 처리한다.<br>
물론 생성된 pom.xml에서 설정 값을 오버라이드하면 super pom.xml 내용을 변경할 수 있다.

##### 참고한 사이트
---

```
https://goddaehee.tistory.com/199
https://jeong-pro.tistory.com/168
```