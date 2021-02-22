---
title:  "[Java] 자료형"
excerpt: "Java 자료형 개념 정리"

categories:
  - Java
tags:
  - [Java, datatype]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

### 자바의 자료형?
---
자바에는 두 가지 성격의 형이 존재<br>
기본형(원시타입, Primitive Type) 과 참조형(Reference Type)으로 구분<br>

### 기본형의 종류
---
- 정수형<br>
byte, 1바이트, -128 ~ 127<br>
short, 2바이트, -32,768 ~ 32,767<br>
int, 4바이트, -2,147,483,648 ~ 2,147,483,647<br>
long, 8바이트, -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807

- 실수형<br>
float, 4바이트, 1.4 x 10^-45 ~ 3.4 x 10^38 (양수범위)<br>
double, 8바이트, 4.9 x 10^-324 ~ 1.8 x 10^308 (양수범위)

- 유니코드<br>
char, 2바이트, 0 ~ 65535

- 논리형<br>
boolean, 미정, false 또는 true

```
! 유니코드

컴퓨터는 오로지 "0"과 "1"만 인식이 가능합니다.
그런데 현실세계에서는 다양한 문자들이 존재합니다.
이런 문자들을 모든 기기에서 사용할 수 있게 각각의 문자들에게  고유번호를 매깁니다.
```

```
! 논리형
Bool 또는 boolean(부울함수 또는 불린이라 부릅니다.)은 참(true), 거짓(false)만을 나타냅니다.
어떤 논리식에 대해서 참인지 거짓인지 나타내기 편한 자료형입니다.
```

### 구분자
---
기본적으로 자바에서는 특별하게 명시한게 없다면 정수의 경우에는 "int형"으로 실수의 경우에는 "double형"으로 취급하게 됩니다.<br>
그래서 long의 경우에 "l"(소문자L) 또는 "L"을 넣어서 long이라는 것을 명시해서 사용해야하며 float의 경우에도 "f" 또는 "F"를 넣어 float이라는 것을 명시해야만 사용이 가능합니다.

### 실수 계산의 오차
---
자바에서는 기본적으로 실수계산의 오차가 있습니다.<br>
그래서 자바에서 제시한 것은 "BigDecimal" 클래스 활용입니다.

### char(캐릭터 자료형 - 문자)
---

```java
public class TestClass{ 
	public static void main(String[] args){ 
		char _char = 'A'; System.out.println(_char); 
	} 
}
```

char형은 문자 하나만 선언이 가능한데 문자는 "작은 따옴표('')"로 감싸게 됩니다.<br>
byte를 자료형으로 선언하여 어떤 변수를 선언하게 되면 1바이트 크기(8비트)의 메모리를 할당<br>

![https://t1.daumcdn.net/cfile/tistory/273D273C58621DC525](https://t1.daumcdn.net/cfile/tistory/273D273C58621DC525)<br>

1바이트는 8비트이므로 8칸을 가지게됩니다.<br>
거기에 3을 저장하는데 2진수로 변하면 00000011로 변하여 저장이 되죠.<br>

그럼 int형으로 변수를 선언하게되면<br>

![https://t1.daumcdn.net/cfile/tistory/2759C93958621DD61B](https://t1.daumcdn.net/cfile/tistory/2759C93958621DD61B)<br>

즉, 각 자료형크기에 맞게 메모리 공간을 할당하게 됩니다.