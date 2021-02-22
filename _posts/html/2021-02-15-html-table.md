---
title:  "[HTML] table 요소"
excerpt: "table 요소 정리"

categories:
  - HTML
tags:
  - [HTML, table]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

### 요소
---
- caption<br>
표의 제목을 의미한다.
- tr<br>
표를 구성하는 각각의 행을 의미한다.<br>
반드시 1개 이상 있어야 한다.
- td<br>
표를 구성하는 각각의 셀을 의미한다.<br>
반드시 1개 이상 있어야 한다.
- th<br>
제목을 의미하는 셀을 나타낸다.
- thead<br>
제목 행들의 집합
- tbody<br>
본문 행들의 집합
- tfoot<br>
요약 행들의 집합
- colgroup<br>
특정한 갯수의 열끼리 묶는다.

### caption
---
테이블 요소 안에서만 위치할 수 있는 자식 요소입니다.<br>
이름 그대로 표 전체의 제목을 나타냅니다.
caption 요소는 테이블 요소의 첫번째 자식이 되어야 합니다.

### tr, td, th
---
tr, td, th는 표를 구성하는 가장 기본이 되는 단위입니다.<br>
이 세 개의 요소들을 활용해 표의 각 행과 그 행을 채우는 각각의 셀(데이터)들을 구성합니다.
- tr<br>
요소는 테이블 안에서 각각의 행을 정의합니다.<br>
자식요소로써 다시 td와 th를 갖습니다.
- td<br>
요소는 표를 구성하는 개개의 셀을 나타냅니다.
- th<br>
요소는 셀 중에서도 제목 역할을 하는 셀을 나타냅니다.

tr 요소로 행을 먼저 정의하고, 그 안에서 td나 th요소를 이용해 각각의 셀들을 정의합니다.

- 제목을 행으로 배치할 경우

```html
<table>
  <caption>
    회원 명부
  </caption>
  <tr>
    <th>이름</th>
    <th>나이</th>
    <th>성별</th>
  </tr>
  <tr>
    <td>홍길동</td>
    <td>20</td>
    <td>남자</td>
  </tr>
</table>
```

- 제목을 열로 배치할 경우

```html
<table>
  <caption>
    스터디 명부
  </caption>
  <tr>
    <th>이름</th>
    <td>홍길동</td>
  </tr>
  <tr>
    <th>나이</th>
    <td>20</td>
  </tr>
  <tr>
    <th>성별</th>
    <td>남자</td>
  </tr>
</table>
```

### thead, tbody, tfoot
---
표의 각 구성요소들을 보다 구조적으로 정의하기 위해 사용하는 요소들입니다.<br>
이 요소들은 tr요소가 나타내는 행들을 감싸서 하나의 집합으로 만들 수 있습니다.

- thead<br>
요소는 표 안에서 제목 역할 을 하는 행들의 집합을 정의할 수 있습니다.
- tbody<br>
요소는 표 안에서 본문 역할 을 하는 행들의 집합을 정의할 수 있습니다.
- tfoot<br>
요소는 표 안에서 요약 역할 을 하는 행들의 집합을 정의할 수 있습니다.

```html
<table>
  <caption>
    회원 명부
  </caption>
  <thead>
    <tr>
      <th>이름</th>
      <th>나이</th>
      <th>성별</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>홍길동</td>
      <td>20</td>
      <td>남자</td>
    </tr>
  </tbody>
  <tfoot>
    <th colspan="2">총원</th>
    <td>1명</td>
  </tfoot>
</table>
```

표를 제목부와 본문부 요약부로 구분하게 될 경우 각각의 부분들을 별도로 스타일링하는데 용이해집니다.

```css
thead, tfoot {
  font-weight: bold;
  background-color: lightgray;
}

tbody {
  font-style: italic;
}
```

### colgroup과 col
---
tbody, thead, tfoot은 여러 행을 그룹핑하기 위한 요소입니다.<br>
이와 반대로 특정한 열 들을 그룹핑하기 위해서 사용하는 것이 바로 colgroup요소입니다.<br>
colgroup 안에 각 열을 대표하는 col요소를 배치합니다.<br>
기본적으로 열의 갯수와 일치하는 수의 col요소를 배치하되, 두 개 이상의 열을 대표하고자 할 때는 span 속성을 활용합니다.

```html
<table>
  <caption>
    운동 시간표
  </caption>
  <colgroup>
    <col />
    <col />
    <col class="day-off" span="2" />
    <col />
  </colgroup>
  <thead>
    <tr>
      <th>월</th>
      <th>화</th>
      <th>수</th>
      <th>목</th>
      <th>금</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>상체</td>
      <td>하체</td>
      <td>쉬는날</td>
      <td>쉬는날</td>
      <td>유산소</td>
    </tr>
    <tr>
      <td>하체</td>
      <td>상체</td>
      <td>쉬는날</td>
      <td>쉬는날</td>
      <td>복근</td>
    </tr>
    <tr>
      <td>복근</td>
      <td>유산소</td>
      <td>쉬는날</td>
      <td>쉬는날</td>
      <td>하체</td>
    </tr>
  </tbody>
</table>
```

행과 열을 그룹핑하는 주된 목적은 표의 구성요소들을 그룹화하여 스타일링하기 위함입니다.

```css
table {
  border-collapse: collapse;
}

.day-off {
  background-color: tomato;
  border: 3px solid;
}
```

### colspan 속성과 rowspan 속성
----
특정 행과 열은 기본적으로 1/n 만큼의 영역을 차지합니다.<br>
colspan과 rowspan 속성을 이용해 1/n 이상의 영역을 차지할 수 있도록 설정할 수 있습니다.<br>
오직 셀 요소들 (td, th) 에서만 사용이 가능한 속성입니다.

```html
<table>
  <caption>
    스터디 명부
  </caption>
  <thead>
    <tr>
      <th colspan="3">구성원</th>
    </tr>
    <tr>
      <th>이름</th>
      <th>나이</th>
      <th>성별</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>최설범</td>
      <td rowspan="2">19</td>
      <td rowspan="2">남자</td>
    </tr>
    <tr>
      <td>주영재</td>
      <!-- <td>19</td> -->
      <!-- <td>남자</td> -->
    </tr>
  </tbody>
  <tfoot>
    <th colspan="2">총원</th>
    <td>2명</td>
  </tfoot>
</table>
```

### scope 속성
---
th 요소는 가로로 펼쳐진 행에 대한 제목이 될 수도 있고, 세로로 나열된 열에 대한 제목이 될 수도 있습니다.<br>
scope 속성을 통해 어떤 제목이 어떤 행에 혹은 열에 관련이 있는지 명시할 수 있습니다.<br>
scope 속성은 그 값으로 row col rowgroup colgroup을 가질 수 있습니다.<br>
rowgroup의 경우 해당 th가 속한 tbody, thead, tfoot과 같은 행 집합에 연결짓는 것입니다.

```html
<table>
  <caption>
    스터디 명부
  </caption>
  <thead>
    <tr>
      <th colspan="3">구성원</th>
    </tr>
    <tr>
      <th scope="col">이름</th>
      <th scope="col">나이</th>
      <th scope="col">성별</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>최설범</td>
      <td rowspan="2">19</td>
      <td rowspan="2">남자</td>
    </tr>
    <tr>
      <td>주영재</td>
      <!-- <td>19</td> -->
      <!-- <td>남자</td> -->
    </tr>
  </tbody>
  <tfoot>
    <th scope="row" colspan="2">총원</th>
    <td>2명</td>
  </tfoot>
</table>
```

### 참고
---

```
https://velog.io/@zuyonze/HTML-Table-%EC%A0%95%EB%A6%AC
```