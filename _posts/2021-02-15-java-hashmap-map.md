---
title:  "[Java] Map, HashMap"
excerpt: "Map, HashMap 개념과 사용법"

categories:
  - Java
tags:
  - [Java, HashMap, Map]

toc: true
classes: wide

sidebar:
  nav: "docs"

date: 2021-02-15
last_modified_at: 2021-02-15
---

### Map이란?
---
자바의 맵(Map)은 대응관계를 쉽게 표현할 수 있게 해 주는 자료형이다.<br>
이것은 요즘 나오는 대부분의 언어들도 갖고 있는 자료형으로 Associative array, Hash라고도 불린다.<br>
맵(Map)은 사전(dictionary)과 비슷하다.<br>
즉, people 이란 단어에 "사람", baseball 이라는 단어에 "야구"라는 뜻이 부합되듯이 Map은 Key와 Value라는 것을 한 쌍으로 갖는 자료형이다.<br>
Map은 리스트나 배열처럼 순차적으로(sequential) 해당 요소 값을 구하지 않고 key를 통해 value를 얻는다.<br>
맵(Map)의 가장 큰 특징이라면 key로 value를 얻어낸다는 점이다.

### 메소드
---
- put<br>

```java
HashMap<String, String> map = new HashMap<String, String>();
map.put("people", "사람");
map.put("baseball", "야구");
```

key와 value가 String 형태인 HashMap을 만들고 위에서 보았던 예제의 항목값들을 입력해 보았다.<br>
key와 value는 위 예제에서 보듯이 put메소드를 이용하여 입력한다.

- get<br>
key에 해당되는 값을 얻기 위해서는 다음과 같이 한다.

```java
map.get("people");
```

위와같이 get 메소드를 이용하면 value값을 얻을 수 있다.

- containsKey<br>
containsKey 메소드는 맵(Map)에 해당 키(key)가 있는지를 조사하여 그 결과값을 리턴한다.

```java
System.out.println(map.containsKey("people"));
```

"people"이라는 키는 존재하므로 true가 출력될 것이다.

- remove<br>
remove 메소드는 맵(Map)의 항목을 삭제하는 메소드로 key값에 해당되는 아이템(key, value)을 삭제한 후 그 value 값을 리턴한다.

```java
System.out.println(map.remove("people"));
```

"people"에 해당되는 아이템(people:사람)이 삭제된 후 "사람"이 출력될 것이다.

- size<br>
size 메소드는 Map의 갯수를 리턴한다.

```java
System.out.println(map.size());
```

"people", "baseball" 두 값을 가지고 있다가 "people"항목이 삭제되었으므로 1이 출력될 것이다.

### HashMap이란?
---
HashMap은 Map 인터페이스를 구현한 대표적인 Map 컬렉션이다.<br>
Map 인터페이스를 상속하고 있기에 Map의 성질을 그대로 가지고 있다.<br>
Key와 value를 묶어 하나의 entry로 저장한다는 특징을 갖는다.<br>
hashing을 사용하기 때문에 많은양의 데이터를 검색하는데 뛰어난 성능을 보인다.

1. Map 인터페이스의 한 종류로 ( "Key", value) 로 이뤄져 있다.
2. key 값을 중복이 불가능 하고 value는 중복이 가능. value에 null값도 사용 가능하다.
3. 멀티쓰레드에서 동시에 HashMap을 건드려 Key - value값을 사용하면 문제가 될 수 있다.<br>멀티쓰레드에서는 HashTable을 쓴다

![Spring_HashMap](/imgsrc/Spring_HashMap.JPG)<br>

위 그림과 같이 HashMap은 내부에 '키'와 '값'을 저장하는 자료 구조를 가지고 있다.<br>
HashMap은 해시 함수를 통해 '키'와 '값'이 저장되는 위치를 결정하므로, 사용자는 그 위치를 알 수 없고, 삽입되는 순서와 들어 있는 위치 또한 관계가 없다. 

### HashMap 선언
---

```java
HashMap<String,String> map1 = new HashMap<String,String>();//HashMap생성
HashMap<String,String> map2 = new HashMap<>();//new에서 타입 파라미터 생략가능
HashMap<String,String> map3 = new HashMap<>(map1);//map1의 모든 값을 가진 HashMap생성
HashMap<String,String> map4 = new HashMap<>(10);//초기 용량(capacity)지정
HashMap<String,String> map5 = new HashMap<>(10, 0.7f);//초기 capacity,load factor지정
HashMap<String,String> map6 = new HashMap<String,String>(){{//초기값 지정
    put("a","b");
}};
```

HashMap을 생성하려면 키 타입과 값 타입을 파라미터로 주고 기본생성자를 호출하면 된다.<br>
HashMap은 저장공간보다 값이 추가로 들어오면 List처럼 저장공간을 추가로 늘리는데 List처럼 저장공간을 한 칸씩 늘리지 않고 약 두배로 늘린다.<br>
여기서 과부하가 많이 발생한다.<br>
그렇기에 초기에 저장할 데이터 개수를 알고 있다면 Map의 초기 용량을 지정해주는 것이 좋다.

### HaspMap 값 넣는 법
---

```java
HashMap<Integer,String> map = new HashMap<>();//new에서 타입 파라미터 생략가능
map.put(1,"사과"); //값 추가
map.put(2,"바나나");
map.put(3,"포도");
```
HashMap에 값을 추가하려면 put(key,value) 메소드를 사용하면 된다.<br>
선언 시 HashMap에 설정해준 타입과 같은 타입의 Key와 Value값을 넣어야 하며 만약 입력되는 키 값이 HashMap 내부에 존재한다면 기존의 값은 새로 입력되는 값으로 대치된다.

### HashMap 값 삭제하는 법
---

```java
HashMap<Integer,String> map = new HashMap<Integer,String>(){{//초기값 지정
    put(1,"사과");
    put(2,"바나나");
    put(3,"포도");
}};
map.remove(1); //key값 1 제거
map.clear(); //모든 값 제거
```

HashMap에 값을 제거하려면 remove(key) 메소드를 사용하면 된다.<br>
오직 키값으로만 Map의 요소를 삭제할 수 있다.<br>
모든 값을 제거하려면 clear() 메소드를 사용하면 된다.

### HashMap 선언 예시
---

```java
HashMap<Integer,String> map = new HashMap<Integer,String>();
map.put(1,"사과");

HashMap<String,String> map2 = new HashMap<String,String>();
map2.put("1번","사과");

HashMap<String,Object> map3 = new HashMap<String,Object>();
map3.put("1번", Object);
```

### KeySet()
---

```java
for(Integer i : map.keySet()){ //저장된 key값 확인
    System.out.println("[Key]:" + i + " [Value]:" + map.get(i)); // 반복문을 돌려서 설정하는 예시
}
```

### Map타입 변수를 생성할 때 HashMap으로 생성하는 경우?
---
다음과 같은 경우가 있다.

```java
Map<String, String> map= new HashMap<>();
```

java.util.Map은 interface이기 때문에 HashMap으로 생성했다.<br>
인터페이스는 선언만 가능하고 객체 생성이 불가능한 것들이다.<br>
그래서 자식인 HashMap으로 객체를 생성한다.<br>
HashMap은 본인의 메소드 외에 부모인Map의 메소드들을 강제 상속받는다.<br>
왜 HashMap을 주로 쓰냐면, Map이 대한 가장 기본적인 구현이 HashMap이기 때문이다.<br>
경우에 따라서 다른 걸 쓰기도 한다.<br>
내가 put한 순으로 엔트리를 순차적으로 탐색을 하고 싶다면 LinkedHashMap, 특정한 기준에 의해서 정렬된 순서로 탐색을 하고 싶다면 TreeMap 등을 사용한다.

```java
HashMap<String, Object> map2= new HashMap<>();
```

이렇게 선언도 가능하다.<br>
interface의 개념이 따로 필요한 부분이다 아래 링크를 참고.<br>
[https://eunrin15.github.io/java/java-interface/]

### 참고
---

```
https://vaert.tistory.com/107
```