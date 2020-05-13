---
title: "백엔드 개념4 - JSTL/EL Servlet/JSP 연동"
excerpt: "JSTL/EL Servlet/JSP 연동"

categories:
 - Back-End
tags:
 - Web

layout: single
---

# EL 사용

- pageContext를 제외한 모든 EL 내장 객체는 Map이다.
- 그러므로 key와 value의 쌍으로 값을 저장하고 있다.
- 기본 문법 : ${ expr }
- EL에서 객체 접근
  - request.setAttribute("usr", "홍길동");
    1. ${requestScope.usr}
    2. ${usr}
  - url?name=홍길동&fruit=사과&fruit=바나나
    1. ${param.name}
    2. &{paramValues.fruit[0]}, ${paramValues.fruit[1]}
  - ${cookie.id.value}
    1. cookie가 null이라면 null return
    2. null이 아니라면 id를 검사 후 null이라면 null return
    3. null이 아니라면 value값 검사
- EL은 값이 Null이라도 Null을 출력하지 않는다.

# JSTL (JSP Standard Tag Library)

- Java EE기반의 웹 애플리케이션 개발 플랫폼을 위한 컴포넌트 모음.

- XML 데이터 처리와 조건문, 반복문을 처리 가능하다.

- 선언 : <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

- <c: ___ >

  | function | tag                     | description                                                  |
  | -------- | ----------------------- | ------------------------------------------------------------ |
  | 변수지원 | set                     | jsp page에서 사용할 변수 설정                                |
  |          | remove                  | 설정한 변수를 제거                                           |
  | 흐름제어 | if                      | 조건에 따른 코드 실행                                        |
  |          | choose, when, otherwise | 다중 조건을 처리할 때 사용                                   |
  |          | forEach                 | array나 collection의 각 항목을 처리할 때 사용                |
  |          | forTokens               | 구분자로 분리된 각각의 토큰을 처리할 때 사용                 |
  | URL처리  | import                  | 웹 어플리케이션의 내부 자원과 외부자원을 포함할 때 사용. <br>한 개 이상의 <c: param> 서브태그를 가짐. |
  |          | redirect                | response.sendRedirect() 기능 <br/>한 개 이상의 <c: param> 서브태그를 가짐. |
  | 기타태그 | catch                   | body에서 실행되는 코드의 예외처리 담당                       |
  |          | out                     | JSP의 Expression Tag(<%= %>)를 대체<br> 값 출력시 사용       |
  