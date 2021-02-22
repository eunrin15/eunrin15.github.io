---
title:  "[Java] 인터페이스(Interface)"
excerpt: "Java Interface 개념 정리"

categories:
  - Java
tags:
  - [Java, Interface]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-16
---

### 인터페이스(interface)란?
---
**인터페이스(interface)란 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스를 의미합니다**.<br>
자바에서 추상 클래스는 추상 메소드뿐만 아니라 생성자, 필드, 일반 메소드도 포함할 수 있습니다.<br>
하지만 **인터페이스(interface)는 오로지 추상 메소드와 상수만을 포함할 수 있습니다**.<br>
또한, **자바에서는 인터페이스라는 것을 통해 다중 상속을 지원하고 있습니다**.

### 인터페이스 선언
---
인터페이스를 선언할 때에는 접근 제어자와 함께 **interface 키워드를 사용**하면 됩니다.

```java
접근제어자 interface 인터페이스이름 {

    public static final 타입 상수이름 = 값;
    ...

    public abstract 메소드이름(매개변수목록);
    ...
}
```

단, 클래스와는 달리 인터페이스의 모든 필드는 **public static final**이어야 하며, 모든 메소드는 **public abstract**이어야 합니다.
이 부분은 모든 인터페이스에 공통으로 적용되는 부분이므로 이 제어자는 생략할 수 있습니다.
이렇게 **생략된 제어자는 컴파일 시 자바 컴파일러가 자동으로 추가**해 줍니다.

### 인터페이스 구현
---
인터페이스는 추상 클래스와 마찬가지로 **자신이 직접 인스턴스를 생성할 수는 없습니다**.
따라서 인터페이스가 포함하고 있는 추상 메소드를 구현해 줄 클래스를 작성해야만 합니다.

```java
class 클래스이름 implements 인터페이스이름 { ... }
```

만약 모든 추상 메소드를 구현하지 않는다면, abstract 키워드를 사용하여 추상 클래스로 선언해야 합니다.

```java
interface Animal { public abstract void cry(); }

class Cat implements Animal {
    public void cry() {
        System.out.println("냐옹냐옹!");
    }
}

class Dog implements Animal {
    public void cry() {
        System.out.println("멍멍!");
    }
}

public class Polymorphism03 {
    public static void main(String[] args) {
        Cat c = new Cat();
        Dog d = new Dog();

        c.cry();
        d.cry();
    }
}
```

```
[실행 결과]
냐옹냐옹!
멍멍!
```

### 상속과 구현을 동시에
---
자바에서는 다음과 같이 상속과 구현을 동시에 할 수 있습니다.

```java
class 클래스이름 extend 상위클래스이름 implements 인터페이스이름 { ... }
```

**인터페이스는 인터페이스로부터만 상속을 받을 수 있으며, 여러 인터페이스를 상속받을 수 있습니다.**


### 인터페이스를 사용한 다중 상속의 예제
---

```java
interface Animal { public abstract void cry(); }
interface Pet { public abstract void play(); }

class Cat implements Animal, Pet {
    public void cry() {
        System.out.println("냐옹냐옹!");
    }

    public void play() {
        System.out.println("쥐 잡기 놀이하자~!");
    }
}

class Dog implements Animal, Pet {
    public void cry() {
        System.out.println("멍멍!");
    }

    public void play() {
        System.out.println("산책가자~!");
    }
}

public class Polymorphism04 {
    public static void main(String[] args) {
        Cat c = new Cat();
        Dog d = new Dog();

        c.cry();
        c.play();
        d.cry();
        d.play();
    }
}
```

```
[실행 결과]
냐옹냐옹!
나비야~ 쥐 잡기 놀이하자~!
멍멍!
바둑아~ 산책가자~!
```
 
Cat 클래스와 Dog 클래스는 각각 Animal과 Pet이라는 두 개의 인터페이스를 동시에 구현하고 있습니다.

### 클래스를 이용한 다중 상속의 문제점
---
클래스를 이용하여 다중 상속을 하면 **메소드 출처의 모호성 등의 문제가 발생**할 수 있습니다.

```java
class Animal { 
    public void cry() {
        System.out.println("짖기!");
    }
}

class Cat extends Animal {
    public void cry() {
        System.out.println("냐옹냐옹!");
    }
}

class Dog extends Animal {
    public void cry() {
        System.out.println("멍멍!");
    }
}

1. class MyPet extends Cat, Dog {}

public class Polymorphism {
    public static void main(String[] args) {
        MyPet p = new MyPet();
2.      p.cry();
    }
}
```
 
위의 예제에서 Cat 클래스와 Dog 클래스는 각각 Animal 클래스를 상속받아 cry() 메소드를 오버라이딩하고 있습니다.<br>
여기까지는 문제가 없지만, 1번 라인에서 MyPet 클래스가 Cat 클래스와 Dog 클래스를 동시에 상속받게 되면 문제가 발생합니다.<br>
2번 라인에서 MyPet 인스턴스인 p가 cry() 메소드를 호출하면, 이 메소드가 Cat 클래스에서 상속받은 cry() 메소드인지 Dog 클래스에서 상속받은 cry() 메소드인지를 구분할 수 없는 모호성을 지니게 됩니다.<br>
이와 같은 이유로 자바에서는 클래스를 이용한 다중 상속을 지원하지 않는 것입니다.<br>
하지만 다음 예제처럼 인터페이스를 이용하여 다중 상속을 하게되면, 위와 같은 메소드 호출의 모호성을 방지할 수 있습니다.

```java
interface Animal { public abstract void cry(); }
interface Cat extends Animal { public abstract void cry(); }
interface Dog extends Animal { public abstract void cry(); }

class MyPet implements Cat, Dog {
    public void cry() {
        System.out.println("멍멍! 냐옹냐옹!");
    }
}

public class Polymorphism05 {
    public static void main(String[] args) {
        MyPet p = new MyPet();
        p.cry();
    }
}
```

```
[실행 결과]
멍멍! 냐옹냐옹!
```

위의 예제에서는 Cat 인터페이스와 Dog 인터페이스를 동시에 구현한 MyPet 클래스에서만 cry() 메소드를 정의하므로, 앞선 예제에서 발생한 메소드 호출의 모호성이 없습니다.

### 인터페이스의 장점
---
인터페이스를 사용하면 다중 상속이 가능할 뿐만 아니라 다음과 같은 장점을 가질 수 있습니다.

1. 대규모 프로젝트 개발 시 **일관되고 정형화된 개발을 위한 표준화가 가능**합니다.
2. 클래스의 작성과 인터페이스의 구현을 동시에 진행할 수 있으므로, **개발 시간을 단축**할 수 있습니다.
3. 클래스와 클래스 간의 관계를 인터페이스로 연결하면, **클래스마다 독립적인 프로그래밍이 가능**합니다.


### 참고
---

```
http://www.tcpschool.com/java/java_polymorphism_interface
```