---
title:  "[Java] Enhanced for문"
excerpt: "Java Enhanced for문 개념 정리"

categories:
  - Java
tags:
  - [Java, Enhanced for, for]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-26
last_modified_at: 2021-02-26
---

### Enhanced for문이란?
---
JDK 1.5부터 **배열과 컬렉션의 모든 요소를 참조하기 위한 반복문**입니다.

### 문법
---

```java
for(타입 변수이름 : 배열이나 컬렉션이름){
  배열의 길이만큼 반복적으로 실행하고자 하는 명령문;
  ...
}
```

Enhanced for 문은 명시한 배열이나 컬렉션의 길이만큼 반복되어 실행됩니다.<br>
루프마다 각 요소는 명시한 변수의 이름으로 저장되며, 명령문에서는 이 변수를 사용하여 각 요소를 참조할 수 있습니다.

### 예시
---

```java
int[] arr = new int[]{1, 2, 3, 4, 5};

for (int e : arr) {
    System.out.print(e + " ");
}
```

```
[실행 결과]
1 2 3 4 5
```

### 주의점
---
Enhanced for 문은 요소를 참조할 때만 사용하는 것이 좋으며, **요소의 값을 변경하는 작업에는 적합하지 않습니다**.

```java
[예제]
int[] arr1 = new int[]{1, 2, 3, 4, 5};
int[] arr2 = new int[]{1, 2, 3, 4, 5};

for (int i = 0; i < arr1.length; i++) {
  arr1[i] += 10;      // 각 배열 요소에 10을 더함.
}

for (int e : arr2) {
  e += 10;            // 각 배열 요소에 10을 더함.
}
```

```
[실행 결과]
11 12 13 14 15
1 2 3 4 5
```

두 가지 반복문을 이용하여 모든 배열 요소에 10을 더하는 예제입니다.<br>
for 문을 활용하여 각 배열 요소에 10을 더하는 것은 문제가 없습니다.<br>
그러나 Enhanced for 문은 원본 배열에 아무런 변화가 없음을 알 수 있습니다.<br>
이렇게 Enhanced for 문 내부에서 사용되는 배열 요소는 배열 요소 그 자체가 아닌 배열 요소의 복사본입니다.<br>
따라서 Enhanced for 문에서 배열 요소의 값을 변경하여도 원본 배열에는 아무런 영향을 주지 못합니다.

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```