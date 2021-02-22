---
title:  "[Spring] Pagination Tag"
excerpt: "Pagination Tag 개념 정리"

categories:
  - Spring
tags:
  - [Java, Spring, Pagination]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-22
last_modified_at: 2021-02-22
---

### Pagination Tag이란?
---

![Spring_Paginationtag](/imgsrc/Spring_Paginationtag.JPG)

페이징 처리의 편의를 위해 전자정부프레임워크에서 제공하는 태그입니다.<br>
페이징 기능을 사용할때 기능은 유사하지만 이미지나 라벨등의 포맷만 다양하게 사용하게 되는 경우가 있다.
따라서 포맷별로 렌더링할 클래스를 빈설정 파일에 설정하고 태그에서 입력된 정보를 기반으로 어떤 렌더링 클래스를 사용할지 결정하는 방식으로 동작한다.
PaginationRenderer는 포맷에 따라 페이징을 렌더링하는 역활을 담당하고, PaginationManager는 어떤 PaginationRenderer를 사용할지를 담당한다.
렌더링에 필요한 데이터는 PaginationInfo에 담겨 있다.
위치 egovframework.rte.ptl.mvc.tags.ui.pagination

### PaginationInfo
---
페이징 처리를 위한 데이터들을 담고 있는 빈 클래스입니다.
Tag 클래스에서 여기 담긴 정보를 기반으로 페이징을 렌더링합니다.
사용자입력여부가 yes인 프로퍼티들은 Controller에서 직접 해당 setter에 값을 넣어줘야 하며, no인 프로퍼티인 값들은 사용자가 입력한 다른 프로퍼티 값으로 자동계산되는 프로퍼티들입니다.

[PaginationInfo의 기본 프로퍼티(필드)]

|이름|설명|사용자입력여부|계산공식|
|:----:|:----|:----:|:----|
|currentPageNo|현재 페이지 번호|yes||
|recordCountPerPage|한 페이지당 게시되는 게시물 건 수|yes||
|pageSize|페이지 리스트에 게시되는 페이지 건수|yes||
|totalRecordCount|전체 게시물 건 수|yes||
|totalPageCount|페이지 개수|no|totalPageCount = ((totalRecordCount-1)/recordCountPerPage) + 1|
|firstPageNoOnPageList|페이지 리스트의 첫 페이지 번호|no|firstPageNoOnPageList = ((currentPageNo-1)/pageSize)*pageSize + 1|
|lastPageNoOnPageList|페이지 리스트의 마지막 페이지 번호|no|lastPageNoOnPageList = firstPageNoOnPageList+pageSize-1<br>if(lastPageNoOnPageList>totalRecordCount){lastPageNoOnPageList=totalPageCount}|
|firstRecordIndex|페이징 SQL의 조건절에 사용되는 시작 rownum|no|firstRecordIndex = (currentPageNo - 1) * recordCountPerPage|
|lastRecordIndex|페이징 SQL의 조건절에 사용되는 마지막 rownum|no|lastRecordIndex = currentPageNo * recordCountPerPage|

### PaginationTag
---
[페이징을 위한 Tag class인 PaginationTag에서 커스텀 태그를 통해 사용자로 부터 입력 받는 프로퍼티]

|이름|설명|필수여부|
|:----|:----|:----:|
|paginationInfo|페이징리스트를 만들기 위해 필요한 데이터.<br>데이터 타입은 egovframework.rte.ptl.mvc.tags.ui.pagination.PaginationInfo이다.|yes|
|type|페이징리스트 렌더링을 담당할 클래스의 아이디.<br>이 아이디는 빈설정 파일에 선언된 프로퍼티 rendererType의 key값이다.	|yes|
|jsFunction|페이지 번호에 걸리게 될 자바스크립트 함수 이름.<br>페이지 번호가 기본적인 argument로 전달된다.|yes|

[PaginationTag의 주요 로직]
- 어떤 PaginationRenderer를 사용할지 PaginationManager에게 위임한다.
- 실제 페이징을 위한 작업은 PaginationManager가 반환한 PaginationRenderer이 담당한다.
- PaginationRenderer가 반환한 String 데이터를 출력한다.

```java
//빈 설정 정보와 type 프로퍼티값으로 PaginationRenderer 구현 클래스 반환.
PaginationRenderer paginationRenderer = paginationManager.getRendererType(type);
//페이징 Contents 반환.
String contents = paginationRenderer.renderPagination(paginationInfo, jsFunction);
//출력.
out.println(contents);
```

### PaginationManager
---
bean 설정 정보와 사용자가 태그에서 입력한 type 프로퍼티값을 기반으로 PaginationManager의 getRendererType 메소드가 PaginationRenderer의 구현 클래스 객체를 반환합니다.

```java
public PaginationRenderer getRendererType(String type);
```

bean 설정이 아래와 같이 되어 있다면,

```xml
<!-- For Pagination Tag -->	 
<bean id="imageRenderer" class="com.easycompany.tag.ImagePaginationRenderer"/>
<bean id="textRenderer" class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationRenderer"/>

<bean id="paginationManager" class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationManager">
    <property name="rendererType">
        <map>
            <entry key="image" value-ref="imageRenderer"/>
            <entry key="text" value-ref="textRenderer"/>
        </map>
    </property>
</bean>
```

사용자가 페이징 기능이 필요한 JSP 페이지에서 아래와 같이 type을 image로 하면 ImagePaginationRenderer가 렌더링을 담당하고,

```jsp
<ui:pagination paginationInfo = "${paginationInfo}" type="image" jsFunction="linkPage"/>
```

type을 text로 하면 DefaultPaginationRenderer이 렌더링을 담당합니다.

```jsp
<ui:pagination paginationInfo = "${paginationInfo}" type="text" jsFunction="linkPage"/>
```

bean 설정이 없거나, 사용자가 입력한 type에 해당하는 PaginationRenderer가 없다면 아래의 두 클래스가 기본값으로 사용됩니다.

```xml
egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationManager
egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationRenderer
```

### PaginationRenderer
---
PaginationInfo의 데이터를 기반으로 페이징을 렌더링하는 역활을 담당합니다.

[주요 메소드]

```java
public String renderPagination(PaginationInfo paginationInfo, String jsFunction);
```

인터페이스 PaginationRenderer의 구현 추상클래스인 AbstractPaginationRenderer는 기본적인 페이징 로직을 제공하고 있습니다.

[AbstractPaginationRenderer의 주요 프로퍼티]

|프로퍼티명|담당할 요소|
|:----|:----|
|firstPageLabel|처음|
|previousPageLabel|이전|
|currentPageLabel|현재 페이지|
|otherPageLabel|현재 페이지를 제외한 다른 페이지|
|nextPageLabel|다음|
|lastPageLabel|마지막|

프로젝트의 요구사항에 따른 Custom PaginationRenderer를 구현할때는 2가지 경우가 있을 것이다.
페이징 로직은 AbstractPaginationRenderer과 동일하나, 각 요소의 포맷만 커스터마이징해야 하는 경우
포맷뿐 아니라 페이징 로직 자체도 커스터 마이징해야 하는 경우
전자인 경우는 주요 프로퍼티들만 오버라이드 하면 된다.
프레임워크에 제공하는 기본 PaginationRenderer인 DefaultPaginationRenderer의 코드는 아래와 같다.

```java
package egovframework.rte.ptl.mvc.tags.ui.pagination;
 
public class DefaultPaginationRenderer extends AbstractPaginationRenderer {
 
	public DefaultPaginationRenderer() {
		firstPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">[처음]</a>&#160;"; 
		previousPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">[이전]</a>&#160;";
		currentPageLabel = "<strong>{0}</strong>&#160;";
		otherPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">{2}</a>&#160;";
		nextPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">[다음]</a>&#160;";
		lastPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">[마지막]</a>&#160;";
	}
}
```

아래와 같이 이미지로 각 요소를 보여 주는 PaginationRenderer인 ImagePaginationRenderer를 만들어 보자.

주요 프로퍼티들만 오버라이드한다.

```java
package com.easycompany.tag;
 
import egovframework.rte.ptl.mvc.tags.ui.pagination.AbstractPaginationRenderer;
 
public class ImagePaginationRenderer extends AbstractPaginationRenderer {
 
	public ImagePaginationRenderer() {
		firstPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\"><image src=\"/easycompany/images/bt_first.gif\" border=0/></a>&#160;"; 
		previousPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\"><image src=\"/easycompany/images/bt_prev.gif\" border=0/></a>&#160;";
		currentPageLabel = "<strong>{0}</strong>&#160;";
		otherPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\">{2}</a>&#160;";
		nextPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\"><image src=\"/easycompany/images/bt_next.gif\" border=0/></a>&#160;";
		lastPageLabel = "<a href=\"#\" onclick=\"{0}({1}); return false;\"><image src=\"/easycompany/images/bt_last.gif\" border=0/></a>&#160;";
	}
}
```

후자인 페이징 로직 자체도 커스터 마이징해야 하는 경우는 메소드 renderPagination을 오버라이드해야 한다.
egovframework.rte.ptl.mvc.tags.ui.pagination.AbstractPaginationRenderer의 renderPagination 메소드를 참고해서 구현 클래스에서 오버라이드하라.

```java
package com.easycompany.tag;
 
import egovframework.rte.ptl.mvc.tags.ui.pagination.AbstractPaginationRenderer;
 
public class ImagePaginationRenderer extends AbstractPaginationRenderer {
 
	public ImagePaginationRenderer() {
		firstPageLabel = "..."; 
		previousPageLabel = "...";
		currentPageLabel = "...";
		otherPageLabel = "...";
		nextPageLabel = "...";
		lastPageLabel = "...";
	}
 
	@Override
	public String renderPagination(PaginationInfo paginationInfo,
			String jsFunction) {		
		...
	}
}
```

### 사용 예제
---
사용예제
사원 리스트를 보여 줄 때, 페이징 처리를 해서 보여주는 예제를 작성해 보겠다.
한 페이지에 3명의 사원을 보여 주고, 페이징은 8개단위로 보여준다.

필요한 작업은 아래와 같다.

페이징 SQL
페이징 SQL 작성은 DBMS 벤더 별로 약간의 차이가 있다.
현재 페이지에 해당하는 게시물만 가져 오기 때문에 구간 index를 줘야 하는데, 벤더별로 차이가 있기 때문이다.
여기서는 MySQL을 사용했다.

```xml
<select id="getAllEmployees" parameterClass="java.util.Map"
    resultClass="com.easycompany.domain.Employee">
    select employeeid,
        name,
        age,
        departmentid,
        password,
        email
    from employee
    <dynamic prepend="WHERE">
        <isNotEmpty prepend="and" property="searchEid">
            employeeid = #searchEid#
        </isNotEmpty>
        <isNotEmpty prepend="and" property="searchDid">
            departmentid = #searchDid#
        </isNotEmpty>
        <isNotEmpty prepend="and" property="searchName">
            name like '%$searchName$%'
        </isNotEmpty>
    </dynamic>
    order by CONVERT(employeeid,SIGNED)
    limit #firstIndex#, #recordCountPerPage#
</select>
```

페이징 연관된 부분은 limit #firstIndex#, #recordCountPerPage# ← 이 부분이다.
이제 DAO와 Service 클래스를 만들어 주면 된다.
DAO와 Service 클래스는 특별한 내용이 없지만 참고가 필요하다면 easycompany 예제에서 아래 코드를 참고하라.

```java
com.easycompany.dao.EmployeeDao.getAllEmployees()
com.easycompany.service.EmployeeService.getAllEmployees()
com.easycompany.service.EmployeeServiceImpl.getAllEmployees()
Controller
페이징 처리를 담당할 Controller를 작성해 보자.

package com.easycompany.controller.annotation;
...
import egovframework.rte.ptl.mvc.tags.ui.pagination.PaginationInfo;
 
@Controller
public class EmployeeListContrller {
 
	@Autowired
	private EmployeeService employeeService;
 
	@RequestMapping(value="/employeeList.do")
	public String getEmpList(@RequestParam(value="pageNo", required=false) String pageNo, ....) throws Exception {
		...
		//PaginationInfo에 필수 정보를 넣어 준다.
		PaginationInfo paginationInfo = new PaginationInfo();
		paginationInfo.setCurrentPageNo(currentPageNo); //현재 페이지 번호
		paginationInfo.setRecordCountPerPage(3); //한 페이지에 게시되는 게시물 건수
		paginationInfo.setPageSize(8); //페이징 리스트의 사이즈
 
		int firstRecordIndex = paginationInfo.getFirstRecordIndex();
		int recordCountPerPage = paginationInfo.getRecordCountPerPage();
		commandMap.put("firstIndex", firstRecordIndex );
		commandMap.put("recordCountPerPage", recordCountPerPage );
 
		List<Employee> employlist = employeeService.getAllEmployees(commandMap);
		...		
		int employeeCount = employeeService.getEmployeeCount(commandMap);
		paginationInfo.setTotalRecordCount(employeeCount); //전체 게시물 건 수
 
		//페이징 관련 정보가 있는 PaginationInfo 객체를 모델에 반드시 넣어준다.
		model.addAttribute("paginationInfo", paginationInfo);
		return "employeelist";	
	}
 
}
```

페이징 빈 설정
빈 설정은 아래와 같이 했다.

```xml
	<!-- For Pagination Tag -->	 
	<bean id="imageRenderer" class="com.easycompany.tag.ImagePaginationRenderer"/>
 
	<bean id="textRenderer" class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationRenderer"/>
 
	<bean id="paginationManager" class="egovframework.rte.ptl.mvc.tags.ui.pagination.DefaultPaginationManager">
		<property name="rendererType">
			<map>
				<entry key="image" value-ref="imageRenderer"/>
				<entry key="text" value-ref="textRenderer"/>
			</map>
		</property>
	</bean>
```

JSP
ui 태그에 대한 라이브러리 선언을 해주고 페이징 리스트가 위치할 곳에 아래와 같이 사용하면 된다.
paginationInfo 속성에는 Controller에서 Model 객체에 저장한 PaginationInfo의 attribute name을 적어 주면 되고,
jsFunction 속성은 페이징 리스트의 각 페이지 번호에 걸릴 링크인 자바스크립트 함수명을 적어 주면 된다.
type 속성은 빈 설정시에 rendererType 프로퍼티의 entry key값을 적어준다. 렌더링 타입을 태그에서 결정하는 것이다.

```jsp
<%@ taglib prefix="ui" uri="http://egovframework.gov/ctl/ui"%>
...
<script type="text/javascript">
	function linkPage(pageNo){
		location.href = "/easycompany/employeeList.do?pageNo="+pageNo;
	}	
</script>
<body>
...
		<ui:pagination paginationInfo = "${paginationInfo}"
			type="image"
			jsFunction="linkPage"/>
...
</body>
```


```
[참고한 사이트]
https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:rte:ptl:view:paginationtag
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```