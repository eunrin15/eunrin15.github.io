---
title:  "[Java] 상수"
excerpt: "Java 상수 개념 정리"

categories:
  - Java
tags:
  - [Java, constant]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

"변하지 않는 값"을 바로 상수라고 합니다.

### 자바에서 상수
---
2가지로 구분됩니다.

1. 리터럴 상수(Literal Constant)
2. 심볼릭 상수(Symbolic Constant)


### 리터럴 상수(Literal Constant)
---

```java
public class TestClass{
    public static void main(String[] args){
        char _char = 65;
        System.out.println(_char);
    }
}
```

바로 65가 리터럴 상수입니다. 위의 예제를 보시면 65는 따로 메모리공간에 넣는 "선언"을 하지 않고 바로 "_char"라는 변수에 넣습니다.<br>
사실 65는 메모리 공간에 들어갔습니다.<br>
하지만 특별한 "이름"이 없이 선언이 되어 올라갔죠.<br>
이러한 것을 리터럴 상수라고 합니다.<br>
즉, 다음과 같은 것들이 전부 리터럴 상수라고 합니다.<br>

![https://t1.daumcdn.net/cfile/tistory/26537833586229C40E](https://t1.daumcdn.net/cfile/tistory/26537833586229C40E)

참고로 리터럴 = 상수, 또는 리터럴 상수라고 합니다.<br>

### 심볼릭 상수(Symbolic Constant)
---
리터럴과는 다르게 심볼릭 상수는 "이름"이 있는 상수입니다.<br>

```java
final 자료형 변수명 = 값;
```

![https://t1.daumcdn.net/cfile/tistory/253DFB38586229DE1C](https://t1.daumcdn.net/cfile/tistory/253DFB38586229DE1C)<br>

num1은 일반 변수고 num2는 앞에 final이 붙었으므로 "상수"가 된겁니다.<br>

```java
package testCom;

public class TestClass {
	public static void main(String[] args) {
		int num1 = 5;
		num1 = 6;
		final int num2 = 6;
		num2 = 10;
	}
}
```

### 상수를 언제 쓰나?
---
변경이 절대로 안되고, 절대로 되면 안되는 변수를 상수로 선언합니다.