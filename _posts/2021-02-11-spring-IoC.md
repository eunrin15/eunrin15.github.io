---
title:  "[Spring] IoC Container"
excerpt: "IoC Container"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, IoC Container]

toc: true
toc_sticky: true
 
date: 2021-02-11
last_modified_at: 2021-02-11
---

### 개요
---
프레임워크의 기본적인 기능인 IoC(Inversion of Control) Container 기능을 제공하는 서비스이다.<br>
객체의 생성 시, 객체가 참조하고 있는 타 객체에 대한 의존성을 소스 코드 내부에서 하드 코딩하는 것이 아닌, 소스 코드 외부에서 설정하게 함으로써, 유연성 및 확장성을 향상시킨다.

### 주요 기능
---
1. Dependency Injection
2. Bean Lifecycle Management

![IoC 개요](/imgsrc/Spring_IoC.jpg)<br>
1. 업무 모듈은 IOC Container 서비스에 객체 생성을 요청한다.
2. IoC Container는 표준 프레임워크 설정 정보에 객체 생성을 위한 종속성 정보 등과 같은 Ioc Container 설정 정보를 참조한다.
3. IoC Container는 설정 정보에 따라 객체를 생성하여 업무 모듈에게 돌려준다.

### IoC(Inversion of Control)이란?
---
IoC는 Inversion of Control의 약자로 한글로 “제어의 역전” 또는 “역제어”라고 부른다.<br>
어떤 모듈이 제어를 가진다는 것은 “어떤 모듈을 사용할 것인지”, “모듈의 함수는 언제 호출할 것인지” 등을 스스로 결정한다는 것을 의미한다.<br>
이러한 제어가 역전되었다는 것은, 어떤 모듈이 사용할 모듈을 스스로 결정하는 것이 아니라 다른 모듈에게 선택권을 넘겨준다는 것을 의미한다.

### DI(Dependency Injection)이란?
---
Dependency Injection이란 모듈간의 의존성을 모듈의 외부(컨테이너)에서 주입시켜주는 기능으로 IoC의 한 종류이다.<br>
런타임 시 사용하게 될 의존대상과의 관계를 Spring Framework 이 총체적으로 결정하고 그 결정된 의존특징을 런타임 시 부여한다.

### Non-IoC/DI vs IoC/DI
---
![Non-IoC/DI_vs_IoC/DI](/imgsrc/Spring_Non_IoC_DI_IoC_DI.JPG)

### 기본 개념
---
1. Container<br>
객체를 생성하고, 객체 간의 의존성을 이어주는 역할을 한다.
2. 설정 정보(Configuration Metadata)<br>
Spring IoC container가 객체를 생성하고, 객체 간의 의존성을 이어줄 수 있도록 필요한 정보를 제공한다.<br>
설정 정보는 기본적으로 XML형태로 작성되며, 추가적으로 Java Annotation을 이용하여서도 설정이 가능하다.
3. Bean<br>
Spring IoC Container에 의해 생성되고 관리되는 객체를 의미한다.

### Container
---
1. BeanFactory
- BeanFactory 인터페이스는 Spring IoC Contatiner의 기능을 정의하고 있는 기본 인터페이스 이다.
- Bean 생성 및 의존성 주입, 생명주기 관리 등의 기능을 제공한다.
2. ApplicationContext
- BeanFactory 인터페이스를 상속받는 ApplicationContext는 BeanFactory가 제공하는 기능 외에 Spring AOP, 메시지 리소스 처리(국제화에 사용됨), 이벤트 처리 등의 기능을 제공한다.
- 모든 ApplicationContext 구현체는 BeanFactory의 기능을 모두 제공하므로, 특별한 경우를 제외하고는 ApplicationContext를 사용하는 것이 바람직하다.
- Spring Framework는 다수의 ApplicationContext 구현체를 제공한다.<br>
다음은 ClassPathXlmApplicationContext를 생성하는 예제이다.<br>

```java
ApplicationContext context = new ClassPathXmlApplicationContext(
    new String[] {"services.xml", "daos.xml"});
Foo foo = (Foo)context.getBean(“foo”);

// an Application is also a BeanFactory (via inheritance)
BeanFactory factory = context;
```
※ Spring Container = Bean Factory = ApplicationContext = DI Container = IoC Container

### XML 설정
---
XML 설정 파일은 <beans/> element를 root로 갖는다.<br>
아래는 기본적인 XML 설정 파일의 모습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->
</beans>
```