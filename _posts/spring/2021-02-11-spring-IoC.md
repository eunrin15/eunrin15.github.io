---
title:  "[Spring] IoC Container"
excerpt: "IoC Container"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, IoC Container]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-11
last_modified_at: 2021-02-15
---

### 개요
---
프레임워크의 기본적인 기능인 IoC(Inversion of Control) Container 기능을 제공하는 서비스이다.<br>
객체의 생성 시, 객체가 참조하고 있는 타 객체에 대한 의존성을 소스 코드 내부에서 하드 코딩하는 것이 아닌, 소스 코드 외부에서 설정하게 함으로써, 유연성 및 확장성을 향상시킨다.

### 주요 기능
---
1. Dependency Injection
2. Bean Lifecycle Management

![IoC 개요](/imgsrc/Spring_IoC.JPG)<br>
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
![Non-IoC/DI vs IoC/DI](/imgsrc/Spring_Non_IoC_DI_IoC_DI.JPG)

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
아래는 기본적인 XML 설정 파일의 모습이다.

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

XML 설정은 여러 개의 파일로 구성될 수 있으며, <import/> element를 사용하여 다른 XML 설정 파일을 import할 수 있다.

```xml
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```

### Bean
---
Bean 정의는 Bean을 객체화하고 의존성을 주입하는 등의 관리를 위한 정보를 담고 있다.<br>
XML 설정에서는 <bean/> element가 Bean 정의를 나타낸다.<br>
Bean 정의는 아래와 같은 속성을 가진다.<br>
![Bean](/imgsrc/Spring_IoC_Bean.JPG)

### Bean 이름
---
모든 Bean은 하나의 id를 가지며, 하나 이상의 name을 가질 수 있다.<br>
id는 contatiner 안에서 고유해야 한다.

```xml
<bean id="exampleBean" class="example.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```

Bean은 <alias/> element를 이용하여 추가적인 name을 가질 수 있다.

```xml
<alias name="fromName" alias="toName"/>
```

### Bean Class
---
모든 Bean은 객체화를 위한 Java Class가 필요하다.(예외적으로 상속의 경우 class가 없어도 된다.)

```xml
<bean id="exampleBean" class="example.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```

### Bean 객체화
---
- 일반적으로 Bean 객체화는 Java 언어의 'new'연산자를 사용한다.<br>
이 경우 별도의 설정은 필요없다.
- 'new' 연산자가 아닌 static factory 메소드를 사용하여 Bean을 객체화할 수 있다.<br>
이 경우 Constructor Injection 방식의 의존성 주입 설정을 따른다.

```xml
<bean id="exampleBean"
    class="examples.ExampleBean"
    factory-method="createInstance"/>
```

자신의 static factory 메소드가 아닌 별도의 Factory 클래스의 static 메소드를 사용하여 Bean을 객체화할 수 있다.<br>
이 경우 역시 Constructor Injection 방식의 의존성 주입 설정을 따른다.

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="com.foo.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="exampleBean"
    factory-bean="serviceLocator"
    factory-method="createInstance"/>
```

```java
ExampleBean exampleBean = new ExampleBean();
ExampleBean exampleBean = ExampleBean.createInstance();
ExampleBean exampleBean = DefaultServiceLocator.createInstance();
```

<bean/> element의 ‘lazy-init’ attribute를 사용하여 Bean 객체화 시기를 설정할 수 있다.

- 일반적으로 Bean 객체화는 BeanFactory가 객체화되는 시점에 수행된다.<br>
만약, ‘lazy-init’ attribute 값이 ‘true’인 경우, 설정된 Bean의 객체가 실제로 필요하다고 요청한 시점에 객체화가 수행된다.
- ‘lazy-init’ attribute가 설정되어 있지 않으면 기본값을 사용한다.<br>
Spring Framework의 기본값은 ‘false’이다.

```xml
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>

<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

<beans/> element의 ‘default-lazy-init’ attribute를 사용하여 XML 설정 파일 내의 모든 Bean 정의에 대한 lazyinit attribute의 기본값을 설정할 수 있다.

```xml
<beans default-lazy-init="true">
    <!-- no beans will be pre-instantiated... -->
</beans>
```

### 의존성 주입
---
의존성 주입에는 Constructor Injection과 Setter Injection의 두가지 방식이 있다.<br>
[ Constructor Injection ]<br>
Constructor Injection은 argument를 갖는 생성자를 사용하여 의존성을 주입하는 방식이다.<br>
constructor-arg element를 사용한다.
생성자의 argument와 constructor-arg element는 class가 같은 것끼리 매핑한다.

```java
package x.y;

public class Foo {
    public Foo(Bar bar, Baz baz) {
        // ...
    }
}
```

```xml
<beans>
    <bean name="foo" class="x.y.Foo">
        <constructor-arg>
            <bean class="x.y.Bar"/>
        </constructor-arg>
    
        <constructor-arg>
            <bean class="x.y.Baz"/>
        </constructor-arg>
    </bean>
</beans>
```

만약 생성자가 같은 class의 argument를 가졌거나 primitive type인 경우 argument와 constructor-arg element간의 매핑이 불가능하다.<br>
이 경우, Type을 지정하거나 순서를 지정할 수 있다.

```java
package examples;
public class ExampleBean {
    // No. of years to the calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }
}
```

- type 지정

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

- 순서 지정

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

[ Setter Injection ]<br>
Setter Injection은 argument가 없는 기본 생성자를 사용하여 객체를 생성한 후, setter 메소드를 사용하여 의존성을 주입하는 방식으로, property element를 사용한다.<br>
Class에 attribute(또는 setter 메소드 명)과 property element의 ‘name’ attribute를 사용하여 매핑한다.

```java
public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested <ref/> element -->
    <property name="beanOne"><ref bean="anotherExampleBean"/></property>
    
    <!-- setter injection using the neater 'ref' attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

매핑 규칙은 property element의 ‘name’ attribute의 첫문자를 알파벳 대문자로 변경하고 그 앞에 ‘set’을 붙인 setter 메소드를 호출한다.

