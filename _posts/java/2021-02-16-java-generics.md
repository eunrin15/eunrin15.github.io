---
title:  "[Java] 제네릭(Generic)"
excerpt: "Java Generic 개념 정리"

categories:
  - Java
tags:
  - [Java, Generic]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-16
last_modified_at: 2021-02-16
---

### 제네릭(generic)이란?
---
자바에서 제네릭(generic)이란 **데이터의 타입(data type)을 일반화한다(generalize)는 것을 의미**합니다.<br>
제네릭은 **클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법**입니다.<br>
이렇게 컴파일 시에 미리 타입 검사(type check)를 수행하면 다음과 같은 장점을 가집니다.

1. 클래스나 메소드 내부에서 **사용되는 객체의 타입 안정성을 높일 수 있습니다**.
2. **반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있습니다**.

### 제네릭의 선언 및 생성
---
클래스와 메소드에만 다음과 같은 방법으로 선언할 수 있습니다.

```java
class MyArray<T> {
    T element;
    void setElement(T element) { this.element = element; }
    T getElement() { return element; }
}
```

위의 예제에서 사용된 'T'를 **타입 변수(type variable)**라고 하며, **임의의 참조형 타입을 의미**합니다.<br>
꼭 'T'뿐만 아니라 어떠한 문자를 사용해도 상관없으며, 여러 개의 타입 변수는 쉼표(,)로 구분하여 명시할 수 있습니다.<br>
**타입 변수는 클래스에서뿐만 아니라 메소드의 매개변수나 반환값으로도 사용할 수 있습니다**.<br>
위와 같이 선언된 제네릭 클래스(generic class)를 생성할 때에는 **타입 변수 자리에 사용할 실제 타입을 명시**해야 합니다.

### 타입 변수로 Integer 타입을 사용하는 예제
---
```java
MyArray<Integer> myArr = new MyArray<Integer>();
```

위처럼 제네릭 클래스를 생성할 때 사용할 실제 타입을 명시하면, **내부적으로는 정의된 타입 변수가 명시된 실제 타입으로 변환되어 처리**됩니다.<br>
자바에서 **타입 변수 자리에 사용할 실제 타입을 명시할 때 기본 타입을 바로 사용할 수는 없습니다**.<br>
이때는 위 예제의 Integer와 같이 **래퍼(wrapper) 클래스를 사용**해야만 합니다.<br>
또한, Java SE 7부터 인스턴스 생성 시 타입을 추정할 수 있는 경우에는 타입을 생략할 수 있습니다.

```java
MyArray<Integer> myArr = new MyArray<>(); // Java SE 7부터 가능함.
```

### 제네릭에서 적용되는 타입 변수의 다형성
---

```java
import java.util.*;

class LandAnimal { public void crying() { System.out.println("육지동물"); } }
class Cat extends LandAnimal { public void crying() { System.out.println("냐옹냐옹"); } }
class Dog extends LandAnimal { public void crying() { System.out.println("멍멍"); } }
class Sparrow { public void crying() { System.out.println("짹짹"); } }

class AnimalList<T> {
    ArrayList<T> al = new ArrayList<T>();

    void add(T animal) { al.add(animal); }

    T get(int index) { return al.get(index); }

    boolean remove(T animal) { return al.remove(animal); }

    int size() { return al.size(); }
}

public class Generic01 {
    public static void main(String[] args) {
        AnimalList<LandAnimal> landAnimal = new AnimalList<>(); // Java SE 7부터 생략가능함.
        landAnimal.add(new LandAnimal());
        landAnimal.add(new Cat());
        landAnimal.add(new Dog());
        // landAnimal.add(new Sparrow()); // 오류가 발생함.

        for (int i = 0; i < landAnimal.size() ; i++) {
            landAnimal.get(i).crying();
        }
    }
}
```

```
[실행 결과]
육지동물
냐옹냐옹
멍멍
```

Cat과 Dog 클래스는 LandAnimal 클래스를 상속받는 자식 클래스이므로, AnimalList<LandAnimal>에 추가할 수 있습니다.<br>
하지만 Sparrow 클래스는 타입이 다르므로 추가할 수 없습니다.

### 제네릭의 제거 시기
---
자바 코드에서 선언되고 사용된 제네릭 타입은 **컴파일 시 컴파일러에 의해 자동으로 검사되어 타입 변환**됩니다.<br>
그리고서 **코드 내의 모든 제네릭 타입은 제거되어, 컴파일된 class 파일에는 어떠한 제네릭 타입도 포함되지 않게 됩니다**.<br>
이런 식으로 동작하는 이유는 제네릭을 사용하지 않는 코드와의 호환성을 유지하기 위해서입니다.

### 타입 변수의 제한
---
제네릭은 '**T**'와 같은 **타입 변수(type variable)를 사용하여 타입을 제한**합니다.<br>
이때 **extends 키워드**를 사용하면 **타입 변수에 특정 타입만을 사용하도록 제한**할 수 있습니다.

```java
class AnimalList<T extends LandAnimal> { ... }
```

위와 같이 클래스의 타입 변수에 제한을 걸어 놓으면 **클래스 내부에서 사용된 모든 타입 변수에 제한이 걸립니다**.<br>
이때에는 클래스가 아닌 인터페이스를 구현할 경우에도 implements 키워드가 아닌 extends 키워드를 사용해야만 합니다.

```java
interface WarmBlood { ... }
...

class AnimalList<T extends WarmBlood> { ... } // implements 키워드를 사용해서는 안됨.
```

클래스와 인터페이스를 동시에 상속받고 구현해야 한다면 엠퍼센트(&) 기호를 사용하면 됩니다.

```java
class AnimalList<T extends LandAnimal & WarmBlood> { ... }
```

```java
import java.util.*;

class LandAnimal { public void crying() { System.out.println("육지동물"); } }
class Cat extends LandAnimal { public void crying() { System.out.println("냐옹냐옹"); } }
class Dog extends LandAnimal { public void crying() { System.out.println("멍멍"); } }
class Sparrow { public void crying() { System.out.println("짹짹"); } }

class AnimalList<T extends LandAnimal> {
    ArrayList<T> al = new ArrayList<T>();

    void add(T animal) { al.add(animal); }

    T get(int index) { return al.get(index); }

    boolean remove(T animal) { return al.remove(animal); }

    int size() { return al.size(); }
}

public class Generic02 {
    public static void main(String[] args) {
        AnimalList<LandAnimal> landAnimal = new AnimalList<>(); // Java SE 7부터 생략가능함.

        landAnimal.add(new LandAnimal());
        landAnimal.add(new Cat());
        landAnimal.add(new Dog());
        // landAnimal.add(new Sparrow()); // 오류가 발생함.

        for (int i = 0; i < landAnimal.size() ; i++) {
            landAnimal.get(i).crying();
        }
    }
}
```

```
[실행 결과]
육지동물
냐옹냐옹
멍멍
```
 
위의 예제는 타입 변수의 다형성을 이용하여 AnimalList 클래스의 선언부에 명시한 'extends LandAnimal' 구문을 생략해도 제대로 동작합니다.<br>
하지만 **코드의 명확성을 위해서는 위와 같이 타입의 제한을 명시하는 편이 더 좋습니다**.

### 제네릭 메소드(generic method)
---
제네릭 메소드란 **메소드의 선언부에 타입 변수를 사용한 메소드를 의미**합니다.<br>
이때 타입 변수의 선언은 메소드 선언부에서 반환 타입 바로 앞에 위치합니다.

```java
public static <T> void sort( ... ) { ... }
```

다음 예제의 제네릭 클래스에서 정의된 타입 변수 T와 제네릭 메소드에서 사용된 타입 변수 T는 전혀 별개의 것임을 주의해야 합니다.

```java
class AnimalList<T> {
    ...

    public static <T> void sort(List<T> list, Comparator<? super T> comp) {
        ...
    }
    ...
}
```

### 와일드카드의 사용
---
와일드카드(wild card)란 **이름에 제한을 두지 않음을 표현하는 데 사용되는 기호를 의미**합니다.<br>
자바의 제네릭에서는 물음표(?) 기호를 사용하여 이러한 와일드카드를 사용할 수 있습니다.

```java
<?>           // 타입 변수에 모든 타입을 사용할 수 있음.
<? extends T> // T 타입과 T 타입을 상속받는 자손 클래스 타입만을 사용할 수 있음.
<? super T>   // T 타입과 T 타입이 상속받은 조상 클래스 타입만을 사용할 수 있음.
```

### 클래스 메소드(static method)인 cryingAnimalList() 메소드의 매개변수의 타입을 와일드카드를 사용하여 제한하는 예제
---

```java
import java.util.*;

class LandAnimal { public void crying() { System.out.println("육지동물"); } }
class Cat extends LandAnimal { public void crying() { System.out.println("냐옹냐옹"); } }
class Dog extends LandAnimal { public void crying() { System.out.println("멍멍"); } }
class Sparrow { public void crying() { System.out.println("짹짹"); } }

class AnimalList<T> {
    ArrayList<T> al = new ArrayList<T>();

    public static void cryingAnimalList(AnimalList<? extends LandAnimal> al) {
        LandAnimal la = al.get(0);
        la.crying();
    }

    void add(T animal) { al.add(animal); }

    T get(int index) { return al.get(index); }

    boolean remove(T animal) { return al.remove(animal); }

    int size() { return al.size(); }
}

public class Generic03 {
    public static void main(String[] args) {
        AnimalList<Cat> catList = new AnimalList<Cat>();
        catList.add(new Cat());

        AnimalList<Dog> dogList = new AnimalList<Dog>();
        dogList.add(new Dog());

        AnimalList.cryingAnimalList(catList);
        AnimalList.cryingAnimalList(dogList);
    }
}
```

```
[실행 결과]
냐옹냐옹
멍멍
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```