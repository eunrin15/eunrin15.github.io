---
title:  "[Spring] MyBatis"
excerpt: "MyBatis 개념 정리"

categories:
  - Spring
tags:
  - [Java, Javascript, Spring, MyBatis]

toc: true
classes: wide

sidebar:
  nav: "docs"
 
date: 2021-02-15
last_modified_at: 2021-02-15
---

### MyBatis 데이터 매퍼 서비스
---
개발자가 작성한 SQL문 혹은 저장프로시저 결과값을 자바 오브젝트에 자동 매핑하는 서비스<br>
수동적인 JDBC 방식의 데이터 처리 작업 코드와는 달리 쿼리결과와 오브젝트 간 자동 매핑을 지원<br>
SQL문과 저장프로시저는 XML 혹은 어노테이션 방식으로 작성 가능

|구성요소|설명|
|:----:|:----|
|MapperConfig<br>XML File|MyBatis 동작을 위한 기본적인 설정을 공통으로 정의|
|Mapper<br>XML File|실행할 SQL문 및 매핑 정보를 XML 방으로 정의|
|Mapper Annotations| 자바 코드 내에서 실행할 SQL문 및 매 정보를 어노테이션을 이용하여 정의|
|Parameter Object|SQL문의 조건절에서 값을 비교하거나 INSERT, UPDATE절 등에서 필요한 입력값을 받아오기 위한 오브젝트|
|Result Object|쿼리 결과를 담아 리턴하기 위한 오브젝트|

### 주요 변경 사항
---
- iBatis의 SqlMapClient > SqlSession 변경<br>
SqlSession 인터페이스<br>
1.MyBatis를 사용하기 위한 기본적인 인터페이스로, SQL문 처리를 위한 메서드를 제공<br>
2.구문 실행 메서드, 트랜잭션 제어 메서드 등 포함<br>
-> selectList(), selectOne(), insert(), update(), delete(), commit(), rollback(), …<br>
3.SqlSessionFactory 클래스를 통해 MyBatis Configuration 정보에 해당 SqlSession 인스턴스를 생성
- 어노테이션 방식 설정 도입<br>
MyBatis는 본래 XML 기반의 프레임워크였으나, Mybatis 3.x 부터 어노테이션 방식의 설정을 지원<br>
Mapper XML File 내 SQL문 및 매핑 정보를, 자바 코드 내에서 어노테이션으로 그대로 적용 가능
- iBatis의 RowHandler > ResultHandler 변경<br>
ResultHandler 인터페이스<br>
1.Result Object에 담겨 리턴된 쿼리 결과를 핸들링할 수 있도록 메서드 제공<br>
2.사용 예시)<br>
대량의 데이터 처리 시, 처리 결과를 File로 출력하고자 할 때 혹은 Result Object의 형태를 Map 형태로 가져올 때

### Migrating from iBatis
---
![Spring_MyBatis_Migrating(1)](/imgsrc/Spring_MyBatis_Migrating(1).JPG)<br>
![Spring_MyBatis_Migrating(2)](/imgsrc/Spring_MyBatis_Migrating(2).JPG)

### MyBatis를 활용한 Persistence Layer 개발
---
[MyBatis 설정 1] SQL Mapper XML 파일 작성 설정<br>
- 실행할 SQL문과 관련 정보 설정
- SELECT/INSERT/UPDATE/DELETE, Parameter/Result Object, Dynamic SQL 등

[MyBatis 설정 2] MyBatis Configuration XML 파일 작성<br>
- MyBatis 동작에 필요한 옵션을 설정
- mapper<br>
SQL Mapper XML 파일의 위치

[스프링연동 설정] SqlSessionFactoryBean 정의<br>
- Spring와 MyBatis 연동을 위한 설정
- 역할) MyBatis 관련 메서드 실행을 위한 SqlSession 객체를 생성
- dataSource, configLocation, mapperLocations 속성 설정

DAO 클래스 작성<br>
- 방법1) SqlSessionDaoSupport를 상속하는 EgovAbstractMapper 클래스를 상속받아 확장/구현<br>
실행할 SQL문을 호출하기 위한 메서드 구현: SQL Mapping XML 내에 정의한 각 Statement id를 매개변수로 전달
- 방법2) DAO 클래스를 Interface로 작성하고, 각 Statement id와 메서드명을 동일하게 작성 (Mapper Interface 방식)<br>
-> Annotation을 이용한 SQL문 작성 가능<br>
-> 메서드명을 Statement id로 사용하기 때문에, 코드 최소화 가능

### [MyBatis 설정 1] SQL Mapper XML 파일 작성
---
실행할 SQL문과 Parameter Object와 Result Object 정보 등을 설정

![Spring_MtBatis_SQL_Mapper](/imgsrc/Spring_MtBatis_SQL_Mapper.JPG)

### [MyBatis 설정 1] SQL Mapper XML 파일 작성 - Dynamic SQL
---
- If<br>
if는 가장 많이 사용되는 Dynamic 요소로, test문의 true, false값에 따라 다양한 조건 설정이 가능<br>
SQL문의 다양한 위치에서 사용 가능하고, 선언된 if 조건에 따라 순서대로 test문을 수행

```xml
<select id="selectEmpList" parameterType=“empVO" resultType=“empVO">
    <![CDATA[
    select EMP_NO as empNo,
        EMP_NAME as empName
    from EMP
    where JOB = ‘Engineer’
    ]]>
    <if test="empNo != null">
        AND EMP_NO = #{empNo}
    </if>
    <if test="empName != null">
        AND EMP_NAME LIKE '%' || #{empName} || '%‘
    </if>
</select>
```

- choose (when, otherwise)<br>
모든 조건을 적용하는 대신 한 가지 조건 만을 적용해야 할 필요가 있는 경우, choose 요소를 사용하며 이는 자바의 switch 구문과 유사한 개념임

```xml
<select id="selectEmpList" parameterType=“empVO" resultType="empVO">
    select *
    from EMP
    where JOB = ‘Engineer’
    <choose>
        <when test=”mgr != null”>
            AND MGR like #{mgr}
        </when>
        <when test=”empNo != null and empName ! =null”>
            AND EMP_NAME like #{empName}
        </when>
        <otherwise>
            AND HIRE_STATUS = ‘Y’
        </otherwise>
    </choose>
</select>
```

- trim (where, set)<br>
AND, OR, ‘,’와 같이 반복되는 문자를 자동적으로 trim(제거)<br>
아래 예제의 <trim prefix=“WHERE” prefixOverrides=“AND|OR”>은 <where>와 동일하게 동작<br>

![Spring_MyBatis_trim](/imgsrc/Spring_MyBatis_trim.JPG)

- foreach<br>
Map, List, Array에 담아 넘긴 값을 꺼낼 때 사용하는 요소

```xml
<select id="selectforForeachFromList" parameterType="map" resultMap="egovMap">
    select
        <!-- List를 넘긴 경우, collection=“list” / Array를 넘긴 경우, collection=“array” 지정 -->
        <foreach collection="list" item="item" separator=", ">
            '${item}' as ${item}
        </foreach>

        <!-- Map 객체에 “collection”이라는 키로 List를 넘긴 경우 -->
        <foreach collection="collection" item="item" separator=", ">
            '${item}' as ${item}
        </foreach>
    from dual
</select>
```

### [MyBatis 설정 2] MyBatis Configuration XML 파일 작성
---
MyBatis 공통 설정 파일로, SqlSession 설정관련 상세 내역을 제어할 수 있는 메인 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-
config.dtd">

<configuration>
    <typeAliases>
        <typeAlias alias="deptVO" type=“x.y.z.service.DeptVO" />
        <typeAlias alias="empVO" type="x.y.z.service.EmpVO" />
    </typeAliases>

    <mappers>
        <mapper resource ="META-INF/sqlmap/mappers/lab-dept.xml" />
        <mapper resource ="META-INF/sqlmap/mappers/lab-emp.xml" />
    </mappers>
</configuration>
```

|요소|설명|
|:----:|:----|
|properties|설정 파일 내에서 ${key} 와 같은 형태로 외부 properties 파일을 참조할 수 있다.|
|settings|런타임시 MyBatis의 행위를 조정하기 위한 옵션 설정을 통해 최적화할 수 있도록 지원한다|
|typeAliases|타입 별칭을 통해 자바타입에 대한 좀더 짧은 이름을 사용할 수 있다. 오직 XML 설정에서만 사용되며, 타이핑을 줄이기 위해 사용 된다.|
|typeHandlers|javaType과 jdbcType 일치를 위해 TypeHandler 구현체를 등록하여 사용할 수 있다.|
|environments|환경에 따라 MyBatis 설정을 달리 적용할 수 있도록 지원한다.|
|Mappers|매핑할 SQL 구문이 정의된 파일을 지정한다|

### [스프링연동 설정] SqlSessionFactoryBean 정의
---
Spring와 MyBatis 연동을 위한 설정으로, MyBatis 관련 메서드 실행을 위해 SqlSession 객체가 필요<br>
스프링에서 SqlSession 객체를 생성하고 관리할 수 있도록, SqlSessionFactoryBean을 정의
- Id와 class는 고정값
- dataSource<br>
스프링에서 설정한 Datasource Bean id를 설정하여 MyBatis가 DataSource를 사용하게 한다.
- configLocation<br>
MyBatis Configuration XML 파일이 위치하는 곳을 설정한다.
- mapperLocations<br>
SQL Mapper XML 파일을 일괄 지정할 수 있다. 단, Configuration 파일에 중복 선언할 수 없
다.

```xml
<!-- SqlSession setup for MyBatis Database Layer -->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="classpath:/META-INF/sqlmap/config/sql-mapper-config.xml" />
    <!-- <property name="mapperLocations" value="classpath:/META-INF/sqlmap/mappers/*.xml" /> -->
</bean>
```

###  MyBatis를 활용한 자바클래스 작성
---
EgovAbstractMapper 클래스를 상속받아 DAO 클래스를 작성<br>

![Spring_MyBatis_EgovAbstractMapper](/imgsrc/Spring_MyBatis_EgovAbstractMapper.JPG)<br>

DAO 클래스 대신 Interface 작성 (Mapper Interface 방식)<br>
- 기존 DAO 클래스의 MyBatis 메서드 호출 코드를 최소화시킨 방법으로, 각 Statement id와 메서드명을 동일하게 작성하면 MyBatis가 자동으로 SQL문을 호출한다.
- 실제 내부적으로 MyBatis는 풀네임을 포함한 메서드명을 Statement id로 사용한다.

![Spring_MyBatis_Mapper_Interface](/imgsrc/Spring_MyBatis_Mapper_Interface.JPG)<br>

- 이 때 SQL Mapper XML 파일의 namespace값을 해당 Mapper의 풀네임으로 설정해야 한다.
- MyBatis는 해당 Mapper의 풀네임과 일치하는 namespace에서 메서드명과 동일한 id를 가진 Statement를 호출한다.
- namespace<br>각 SQL Mapper XML을 구분
- [SQL Mapper XML 설정 1]

```xml
<mapper namespace=“x.y.z.mapper.DeptMapper">
    <insert id="insertDept" parameterType="deptVO">…</insert>
    <update id="updateDept" parameterType="deptVO">…</update>
    <delete id="deleteDept" parameterType="deptVO">…</delete>
    <select id="selectDept" parameterType="deptVO" resultMap="deptResult">…</select>
    <select id="selectDeptList" parameterType="deptVO" resultMap="deptResult">…</select>
</mapper>
```

- [SQL Mapper XML 설정 2]

```xml
<mapper namespace=“x.y.z.mapper.EmpMapper">
    <insert id="insertEmp" parameterType=“empVO">…</insert>
    <update id="updateEmp" parameterType="empVO”>…</update>
    <delete id="deleteEmp" parameterType="empVO”>…</delete>
    <select id="selectEmp" parameterType="empVO" resultMap=“empResult">…</select>
    <select id="selectEmpList" parameterType="empVO" resultMap=“empResult">…</select>
</mapper>
```

- @Mapper를 사용하여 Mapper Interface가 동작하도록 하려면, MapperConfigurer 클래스를 빈으로 등록한다.
- MapperConfigurer는 @Mapper를 자동 스캔하고, MyBatis 설정의 편리함을 제공한다.
- basePackage<br>
스캔 대상에 포함시킬 Mapper Interface가 속한 패키지를 지정

```xml
<!-- MapperConfigurer setup for MyBatis Database Layer -->
<bean class="egovframework.rte.psl.dataaccess.mapper.MapperConfigurer">
    <property name="basePackage" value=“x.y.z.mapper" />
</bean>
```

DAO 클래스 대신 Interface 작성 (Mapper Interface 방식) – 어노테이션을 이용한 SQL문 작성<br>
- 인터페이스 메소드 위에 @Statement(Select, Insert, Update, Delete …)를 선언하여 쿼리를 작성한다.
- SQL Mapper XML을 작성할 필요가 없으나, Dynamic 쿼리를 사용하지 못하고 쿼리의 유연성이 떨어진다.

```java
@Mapper("deptMapper")
public interface DeptMapper {

    @Select("select DEPT_NO as deptNo, DEPT_NAME as deptName, LOC as loc from DEPT where DEPT_NO =
    #{deptNo}")
        public DeptVO selectDept(BigDecimal deptNo);

    @Insert("insert into DEPT(DEPT_NO, DEPT_NAME, LOC) values (#{deptNo}, #{deptName}, #{loc})")
        public void insertDept(DeptVO vo);

    @Update("update DEPT set DEPT_NAME = #{deptName}, LOC = #{loc} WHERE DEPT_NO = #{deptNo}")
        public int updateDept(DeptVO vo);

    @Delete("delete from DEPT WHERE DEPT_NO = #{deptNo}")
        public int deleteDept(BigDecimal deptNo);
}
```

```
공부하고 참고하여 기록해두는 개인 기록용 포스팅입니다!
🤔 부족한 부분이 많으니 감안하여 봐주시길 바랍니다. 🤔
```