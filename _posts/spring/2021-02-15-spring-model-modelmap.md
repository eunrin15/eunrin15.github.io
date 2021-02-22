---
title:  "[Spring] Model, ModelMap, ModelAndView"
excerpt: "Model, ModelMap, ModelAndView 차이점 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, Model, ModelMap, ModelAndView]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-22
---

### Model, ModelMap Vs ModelAndView 차이점
---
데이터만 저장 vs 데이터와 이동하고자 하는 View Page를 같이 저장

### Model, ModelMap 공통점
---

```java
model.addAttribute("변수명");

modelMap.addAttribute("변수명");
```

둘 다 addAttribute를 사용한다.<br>
Model or ModelMap에 데이터만 저장 후 View에서 사용목적

### Model, ModelMap 차이점
---
1. Model > 인터페이스
2. ModelMap > 클래스
 

- Java Controller

```java
@RequestMapping(value = "/test.do")
public String test(HttpServletRequest request, Model model, ModelMap modelMap){
        
    String modelStr = "Model Test";
    String modelMapStr = "ModelMap Test";
    
    model.addAttribute("modelVar", modelStr);
    model.addAttribute("modelMapVar", modelMapStr);
        
    return "temp/test";
}
```

- JSP

```jsp
<body>
    Model 저장한 값 : <input type="text" value="${modelVar }"/><br/>
    ModelMap 저장한 값 : <input type="text" value="${modelMapVar }"/>
</body>
```

### ModelAndView
---
addObject를 통해 데이터를 저장한다.<br>
setViewName을 통해 이동하고자 하는 View를 저장한다.<br>
메소드 안에서

```java
ModelAndView mv = new ModelAndView(); 

return type ModelAndView
```

- Java Controller

```java
@RequestMapping(value = "/test.do")
public ModelAndView test(HttpServletRequest request, ModelAndView mv){
        
    String modelAndViewStr = "ModelAndView Test";
    
    mv.addObject("modelAndViewVar", modelAndViewStr);
    mv.setViewName("temp/test");
        
    return mv;
}
```

- JSP

```jsp
<body>
    ModelAndView 저장한 값 : <input type="text" value="${modelAndViewVar }"/><br/>
</body>
```

### 관련 포스팅
---
[**ModelMap**](https://eunrin15.github.io/spring/spring-ModelMap)

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```