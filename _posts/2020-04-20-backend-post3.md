---
title: "백엔드 개념3 - Session/Cookie"
excerpt: "Session&Cookie, MVC"

categories:
 - Back-End
tags:
 - Web

layout: single
---

# Session & Cookie

- http protocol의 특징

  - client가 server에 요청

  - server는 요청에 대한 처리를 한 후 client에 응답

  - 응답 후 연결을 해제 >> stateless

    -> 지속적인 연결로 인한 자원낭비를 줄이기 위해 연결을 해제한다

    -> 그러나 Client와 Server가 연결 상태를 유지해야 하는 경우 문제가 발생 (로그인 정보 등)
    
    -> 즉, Client 단위로 상태 정보를 유지해야 하는 경우 Cookie와 Session이 사용된다.

## 특징

|           | Session                                                      | Cookie                                                       |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Type      | javax.servlet.http.HttpSession(Interface)                    | javax.servlet.http.Cookie(Class)                             |
| 저장 위치 | server의 memory에 Object로 저장.                             | client 컴퓨터에 file로 저장                                  |
| 저장 형식 | Object는 모두 가능. (일반적으로 Dto, List 등 저장.)          | file에 저장되기 때문에 String 형태                           |
| 사용 예   | 로그인 시 사용자 정보, 장바구니 등                           | 최근 본 목록, 아이디 저장(자동로그인), 팝업 메뉴에서 '오늘은 그만 열기' 등 |
| 공통      | 전역에 저장하기 때문에 Project내의 모든 JSP에서 사용가능<br />Map 형식으로 관리하기 때문에 key값의 중복을 허용하지 않는다.  |

- Cookie는 비휘발성이다. 컴퓨터에 파일로 저장되어있다.
- 랜덤한 session id를 생성해서 쿠키에 저장해놓는다. 그 후 나중에 session id를 통하여 최근 본 목록 등을 출력할 수 있는 것이다.

## HttpSession의 주요 기능

| 기능             | method                                                       |
| ---------------- | ------------------------------------------------------------ |
| 생성             | HttpSession session = request.getSession();<br />HttpSession session = request.getSesstion(false); |
| 값 저장          | session.setAttribute(String name, Object value);             |
| 값 얻기          | Object obj = session.getAttribute(String name);              |
| 값 제거          | session.removeAttribute(String name);<br />session.invalidate(); // logout할 때! |
| 생성시간         | long ct = session.getCreationTime();                         |
| 마지막 접근 시간 | long lt = session.getLastAccessedTime();                     |

- new Session이 아니라 getSession()
- session persistence : context.xml에서 설정.

## Cookie의 주요 기능

| 기능                          | method                                                       |
| ----------------------------- | ------------------------------------------------------------ |
| 생성                          | Cookie cookie = new Cookie(String name, String value);       |
| 값 변경/ 얻기                 | cookie.setValue(String value);<br />String value = cookie.getValue(); |
| 사용 도메인 지정/얻기         | cookie.setDomain(String domain);<br />String domain = cookie.getDomain(); |
| 값 범위 지정/얻기             | cookie.setPath(String path);<br />String path = cookie.getPath(); |
| cookie의 유효기간지정/ 얻기   | cookie.setMaxAge(int expiry);<br />int expiry = cookie.getMaxAge();<br />cookie삭제 : cookie.setMaxAge(0); |
| 생성된 cookie를 client에 전송 | response.addCookie(cookie);                                  |
| client에 저장된 cookie        | Cookie cookies[] = request.getCookies();                     |

# Web Application Model

- JSP를 이용하여 구성할 수 있는 Web Application Architecture는 크게 model1과 model2로 나뉜다.
- JSP가 Client의 요청에 대한 Logic 처리와 response page(view)에 대한 처리를 다 하느냐 (model1), 아니면 response page(view)에 대한 처리만 하는가(model2)가 가장 큰 차이점이다.
- Model2구조는 MVC(Model-View-Controller) Pattern을 web 개발에 도입한 구조를 말한다.

## Model1 구조

<img width="1111" alt="model1_architecture" src="https://user-images.githubusercontent.com/33771279/79706386-0b36e700-82f4-11ea-8fc8-36bc3e1c526e.png">



## Model2 구조

<img width="1394" alt="model2_architecture" src="https://user-images.githubusercontent.com/33771279/79706401-1853d600-82f4-11ea-877f-f16d62cea562.png">

- 서블릿은 데이터를 받고 일처리는 모델단에서 한다. 그래서 서블릿은 controller!
- Entity : 값을 가지고 있는 객체
- JSP는 화면에 출력하는 용도로만 사용하는 것이 좋다. JSP에 자바 코드가 적을수록 이상적인 JSP코드의 모습이다. (MVC중에서 View에 해당한다.)
- DAO (Database Access Object)

# MVC(Model-View-Controller) Pattern

## Model

- Business Service Object : service는 business logic을 주로 수행한다.
- Database Access Object

## View

- JSP

## Controller

- Servlet