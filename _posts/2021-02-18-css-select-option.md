---
title:  "[HTML] select와 option을 이용한 드롭다운 리스트 만들기"
excerpt: "select와 option 설명 및 사용법"

categories:
  - HTML
tags:
  - [HTML, select, option]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-18
last_modified_at: 2021-02-18
---

### 문제점
---
옵션 메뉴가 있는 드롭다운 리스트를 만들고 싶다.

### <select> 태그
---
select 태그는 옵션 메뉴를 제공하는 드롭다운 리스트(drop-down list)를 정의할 때 사용합니다.

|:----:|:----:|:----:|
|속성명|속성값|설명|
|autofocus|autofocus|페이지가 로드될 때 자동으로 포커스(focus)가 드롭다운 리스트로 이동됨을 명시함.|
|disabled|disabled|해당 드롭다운 리스트가 비활성화됨을 명시함.|
|form|form id|해당 드롭다운 리스트가 포함될 하나 이상의 form 요소를 명시함.|
|multiple|multiple|사용자가 한 번에 두 개 이상의 옵션을 선택할 수 있음을 명시함.|
|name|이름|드롭다운 리스트의 이름을 명시함.|
|required|required|폼 데이터(form data)가 서버로 제출되기 전 사용자가 반드시 드롭다운 리스트의 값을 선택해야 함을 명시함.|
|size|숫자|드롭다운 리스트에서 한 번에 보일 옵션의 개수를 명시함.|

### <option> 태그
---
option 태그는 옵션 메뉴를 제공하는 드롭다운 리스트(drop-down list)에서 사용되는 하나의 옵션을 정의할 때 사용합니다.<br>
이러한 option 요소는 select 요소나 datalist 요소 내부에만 위치할 수 있습니다.<br>
option 요소는 아무런 속성도 명시하지 않은 채 사용할 수 있지만, 서버로 제출되는 값을 명시하는 value 속성은 명시하는 것이 일반적입니다.<br>
만약 옵션의 개수가 많다면, optgroup 요소를 사용하여 관련된 옵션들을 좀 더 보기 좋게 서로 묶어 줄 수 있습니다.

```html
<select>
    <option value="americano">아메리카노</option>
    <option value="caffe latte">카페라테</option>
    <option value="cafe au lait">카페오레</option>
    <option value="espresso">에스프레소</option>
</select>
```

|:----:|:----:|:----:|
|속성명|속성값|설명|
|disabled|disabled|해당 옵션이 비활성화됨을 명시함.|
|label|텍스트|해당 옵션의 라벨(label)을 명시함.|
|selected|selected|페이지가 로드될 때 옵션 중에서 미리 선택되어지는 옵션을 명시함.|
|value|값|해당 옵션이 선택될 때 서버로 제출되는 값을 명시함.|