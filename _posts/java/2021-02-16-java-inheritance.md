---
title:  "[Java] 상속(inheritance)"
excerpt: "Java inheritance 개념 정리"

categories:
  - Java
tags:
  - [Java, inheritance]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-16
last_modified_at: 2021-02-16
---

### 상속(inheritance)이란?
---
**기존의 클래스에 기능을 추가하거나 재정의하여 새로운 클래스를 정의하는 것을 의미**<br>
이러한 상속은 캡슐화, 추상화와 더불어 객체 지향 프로그래밍을 구성하는 중요한 특징 중 하나입니다.<br>
상속을 이용하면 기존에 정의되어 있는 클래스의 모든 필드와 메소드를 물려받아, 새로운 클래스를 생성할 수 있습니다.<br>
이때 기존에 정의되어 있던 클래스를 부모 클래스(parent class) 또는 상위 클래스(super class), 기초 클래스(base class)라고도합니다.<br>
그리고 상속을 통해 새롭게 작성되는 클래스를 자식 클래스(child class) 또는 하위 클래스(sub class), 파생 클래스(derived class)라고도 합니다.

### 상속의 장점
---
1. 기존에 작성된 클래스를 재활용할 수 있습니다.
2. 자식 클래스 설계 시 중복되는 멤버를 미리 부모 클래스에 작성해 놓으면, 자식 클래스에서는 해당 멤버를 작성하지 않아도 됩니다.
3. 클래스 간의 계층적 관계를 구성함으로써 다형성의 문법적 토대를 마련합니다.

### 자식 클래스(child class)
---
**부모 클래스의 모든 특성을 물려받아 새롭게 작성된 클래스를 의미**<br>
자바에서 자식 클래스는 다음과 같은 문법을 통해 선언합니다.

```java
class 자식클래스이름 extend 부모클래스이름 { ... }
```
 
### 부모 클래스와 자식 클래스 간의 포함 관계
---

![Spring_inheritance_child](/imgsrc/Spring_inheritance_child.png)

이처럼 부모 클래스는 자식 클래스에 포함된 것으로 볼 수 있습니다.<br>
따라서 부모 클래스에 새로운 필드를 하나 추가하면, 자식 클래스에도 자동으로 해당 필드가 추가된 것처럼 동작합니다.<br>
자식 클래스에는 부모 클래스의 필드와 메소드만이 상속되며, 생성자와 초기화 블록은 상속되지 않습니다.<br>
또한, 부모 클래스의 접근 제어가 private이나 default로 설정된 멤버는 자식 클래스에서 상속받지만 접근할 수는 없습니다.

```java
class Parent {
    private int a = 10; // private 필드
    public int b = 20;  // public 필드
}

class Child extends Parent {
    public int c = 30;  // public 필드
    void display() {
1.    // System.out.println(a); // 상속받은 private 필드 참조
2.    System.out.println(b);    // 상속받은 public 필드 참조
3.    System.out.println(c);    // 자식 클래스에서 선언한 public 필드 참조
    }
}

public class Inheritance01 {
    public static void main(String[] args) {
        Child ch = new Child();
        ch.display();
    }
}
```

```
[실행 결과]
20
30
```

위 예제의 2번 라인에서는 자식 클래스의 메소드에서 부모 클래스에서 상속받은 public 필드를 참조하고 있습니다.<br>
이처럼 자식 클래스에서 따로 선언하지 않은 필드라도 해당 이름의 필드를 부모 클래스에서 상속받았다면 문제가 없습니다.<br>
하지만 주석 처리된 1번 라인처럼 해당 필드가 부모 클래스의 private 필드라면 접근할 수 없으므로, 오류를 발생시킬 것입니다.<br>
또한, 자식 클래스에서는 3번 라인처럼 자신만의 필드나 메소드를 선언하여 사용할 수 있습니다.<br>
자바에서 클래스는 단 한 개의 클래스만을 상속받는 단일 상속만이 가능합니다.

### Object 클래스
---
자바에서 Object 클래스는 모든 클래스의 부모 클래스가 되는 클래스입니다.<br>
따라서 자바의 모든 클래스는 자동으로 Object 클래스의 모든 필드와 메소드를 상속받게 됩니다.<br>
즉, 자바의 모든 클래스는 별도로 extends 키워드를 사용하여 Object 클래스의 상속을 명시하지 않아도 Object 클래스의 모든 멤버를 자유롭게 사용할 수 있습니다.<br>
자바의 모든 객체에서 toString()이나 clone()과 같은 메소드를 바로 사용할 수 있는 이유가 해당 메소드들이 Object 클래스의 메소드이기 때문입니다.

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```