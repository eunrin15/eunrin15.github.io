---
title:  "[Spring] ModelMap"
excerpt: "ModelMap 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, ModelMap]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-22
last_modified_at: 2021-02-22
---

### ModelMap이란?
---
java.util.Map< java.lang.String,java.lang.Object >, atg.service.actor.Model을 상속받습니다.<br>
@RequestMapping 메서드로 Model, ModelMap을 리턴하는 경우 모델 데이터가 뷰(view)에 전달됩니다.

### 메소드
---

|타입|메소드명|설명|원문|
|:----:|:----:|:----|:----|
|java.lang.Object|getAttribute(java.lang.String pChildKey, atg.service.actor.Model.AttributeKey pAttributeKey)|하위 모델 개체의 속성 값을 가져옵니다.|Gets a value for an attribute on a child Model object.|
|java.lang.Object|getSubValue(java.lang.String pChildKey)|자식 또는 손자 손녀에서 값을 가져옵니다.<br>손녀에 액세스하기 위해 점 및 배열 표기법을 지원합니다.|Gets a value from a child or grand child - supports dot and array notation to access grand children.|
|void|putSubValue(java.lang.String pChildKey, java.lang.Object pValue)|자식 또는 손자 손녀에 값을 입력합니다.<Br>손녀에 액세스할 수 있도록 점 및 배열 표기법을 지원합니다.|Put a value into a child or grand child - supports dot and array notation to access grand children.|
|void|setAttribute(java.lang.String pChildKey, atg.service.actor.Model.AttributeKey pAttributeKey, java.lang.Object pValue)|하위 모델 개체에 대한 속성 값을 설정합니다.|Sets a value for an attribute on a child Model object.|

### 사용 예시
---

```java
Import org.springframework.ui.ModelMap;

@RequestMapping(value = "/test.do")
public ModelAndView Main(ModelMap model) {
    String str = "문자열전달하기";
    model.addAttribute("strValue", str);

    return "sample/testPage";
}
```

```jsp
[jsp에서 사용법]

"${strValue}"
```

### 관련 포스팅
---
[**Model,ModelMap,ModelAndView**](https://eunrin15.github.io/spring/spring-model-modelmap)

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```