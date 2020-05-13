---
title: "백엔드 개념2 - JSP"
excerpt: "JSP, JSP 기본객체"

categories:
 - Back-End
tags:
 - Web
 - JSP

layout: single
---

# JSP (Java Server Page)

- 웹 페이지를 동적으로 처리할 수 있는 기술 중의 하나로 서블릿 프로그램의 기능을 HTML 파일 내에 스크립트 형식으로 구현할 수 있다.

- **JSP 실행 시에는 자바 서블릿으로 변환된 후 실행된다.**

## JSP 스크립팅 요소 (Scripting Element)

1. 선언 (Declaration)

   - 멤버변수 선언이나 메소드를 선언하는 영역
   - 형식 : <%! 선언 %>

   ```jsp
   <%!
     String name;
   
   	public void init(){
       name = "soohyun";
     }
   %>
   ```

   ```jsp
   <%! List<BoardDto> boardList = new ArrayList<BoardDto>(); %>
   <%
   	for(int i=0;i<10;i++){
   		BoardDto boardDto = new BoardDto();
   		boardDto.setUsername("name"+i);
   		boardDto.setTitle("title"+i);
   		boardList.add(boardDto);
   	}
   %>
   ```

   ```jsp
   <body>
   <div class="container">
   <br>
     <h2>Basic Table</h2>
     <p>The .table class adds basic styling (light padding and horizontal dividers) to a table:</p>            
     <table class="table">
       <thead>
         <tr>
           <th>이름</th>
           <th>제목</th>
         </tr>
       </thead>
       <tbody>
   <%
   	for(BoardDto dto : boardList){
   %>
         <tr>
           <td><%= dto.getUsername() %></td>
           <td><% out.print(dto.getTitle()); %></td>
         </tr>
   <%
   	}
   %>
       </tbody>
     </table>
   </div>
   </body>
   ```

   - 호출될 때마다 boardList가 늘어난다.

2. 스크립트릿 (Scriptlet)

   - Client 요청 시 매번 호출되는 영역이다.
   - Servlet으로 변환시 service() method에 해당되는 영역.
   - request, response에 관련된 코드 구현.
   - 형식 : <% java code %>

   ```jsp
   <%
   	List<BoardDto> boardList = new ArrayList<BoardDto>();
   	for(int i=0;i<10;i++){
   		BoardDto boardDto = new BoardDto();
   		boardDto.setUsername("name"+i);
   		boardDto.setTitle("title"+i);
   		boardList.add(boardDto);
   	}
   %>
   ```

   ```jsp
   <body>
   <div class="container">
   <br>
     <h2>Basic Table</h2>
     <p>The .table class adds basic styling (light padding and horizontal dividers) to a table:</p>            
     <table class="table">
       <thead>
         <tr>
           <th>이름</th>
           <th>제목</th>
         </tr>
       </thead>
       <tbody>
   <%
   	for(BoardDto dto : boardList){
   %>
         <tr>
           <td><%= dto.getUsername() %></td>
           <td><% out.print(dto.getTitle()); %></td>
         </tr>
   <%
   	}
   %>
       </tbody>
     </table>
   </div>
   </body>
   ```

   - 0~9까지 한번의 boardList만 존재한다.

3. 표현식 (Expression)
   - 데이터를 브러우저에 출력할 때 사용
   - 형식 : <$= 문자열 %>
   - 문자열 뒤에 세미콜론은 작성하지 않는다.
   - <% out.print(문자열); %> 와 같은 표현이다.

4. 주석 (Comment)
   - 코드 상에서 부가 설명을 작성
   - 형식 : <%-- 주석 --%>
   - \<!-- html 주석 -->

## JSP 지시자 (Directive)

1. page Directive

   - 컨테이너에게 현재 JSP페이지를 어떻게 처리할 것인가에 대한 정보를 제공한다.
   - 형식 : <%@ page attr="..." %>

   | 속성(attr)   | 기본값                        | 설명                                                         |
   | ------------ | ----------------------------- | ------------------------------------------------------------ |
   | language     | java                          | 스크립트에서 사용할 언어 지정                                |
   | info         |                               | 현재 JSP 페이지에 대한 설명                                  |
   | contentType  | text/html;charset=ISO-8859-1  | 브라우저로 내보내는 내용의 MIME형식 지정 및 문자 집합 지정   |
   | pageEncoding | ISO-8859-1                    | 현재 JSP 페이지 문자집합 지정                                |
   | import       |                               | 현재 JSP 페이지에서 사용할 JAVA 패키지나 클래스를 지정       |
   | session      | true                          | 세션의 사용 유무 설정                                        |
   | errorPage    |                               | 에러가 발생할 때에 대신 처리 될 JSP 페이지 지정              |
   | isErrorPage  | false                         | 현재 JSP 페이지가 여러 핸들링하는 페이지인지 지정하는 요소   |
   | buffer       | 8 KB                          | 버퍼의 크기                                                  |
   | autoflush    | true                          | 버퍼의 내용을 자동으로 브라우저로 보낼 지에 대한 설정        |
   | isThreadsafe | true                          | 현재 JSP 페이지가 멀티 쓰레드로 동작해도 안전한지 여부를 설정하는 것으로 false인 경우 JSP 페이지는 SingleThread로 서비스 된다. |
   | extends      | javax.servlet.jsp.HttpJspPage | 현재 JSP페이지를 기본적인 클래스가 아닌 다른 클래스로부터 상속하도록 변경 |

2. include Directive

   - 특정 jsp file을 페이지에 포함.
   - 여러 jsp페이지에서 반복적으로 사용되는 부분을 jsp file로 만든 후 반복 영역에 include 시켜 반복되는 코드를 줄일 수 있다.
   - 형식 : <%@ include %>
   - Header나 Footer에 많이 사용하는 듯 하다.

3. taglib Directive

## JSP 기본객체

- 표현식(Expression), 스크립트릿(Scriptlets)에서 코드를 심플하게 만들어 주기 위해서 지원
- 서블릿 패키지 내의 클래스 혹은 인터페이스라고 볼 수 있다.

| 기본 객체명 | Return Type         | 설명                                                         |
| ----------- | ------------------- | ------------------------------------------------------------ |
| request     | HttpServletRequest  | HTML 폼 요소의 선택 값 등 사용자 입력 정보를 읽어올 때 사용  |
| response    | HttpServletResponse | 사용자 요청에 대한 응답을 처리하기 위해 사용                 |
| pageContext | pageContext         | 각종 기본 객체를 얻거나 forward 및 include 기능을 활용할 때 사용 |
| session     | HttpSession         | 클라이언트에 대한 세션 정보를 처리하기 위해 사용             |
| application | ServletContext      | 웹 서버의 어플리케이션 처리와 관련된 정보를 레퍼런스 하기 위해 사용 |
| out         | JspWriter           | 사용자에게 전달하기 위한 output 스트림을 처리할 때 사용      |
| config      | ServletConfig       | 현재 JSP에 대한 초기화 환경을 처리하기 위해 사용             |
| page        | java.lang.Object    | 현재 JSP 페이지에 대한 참조 변수에 해당됨                    |
| exception   | java.lang.Exception | Error를 처리하는 JSP에서 isErrorPage 속성을 true로 설정하면 exception 내장 객체를 사용할 수 있고 기본은 false로 설정되어 있다. 전달된 오류 정보를 담고 있는 내장 객체 |

### 활성 범위(Scope)와 기본객체

| Scope       | 연관기본객체 | 생성시기                                       | 소멸시기                                                     |
| ----------- | ------------ | ---------------------------------------------- | ------------------------------------------------------------ |
| page        | pageContext  | JSP 페이지 처리 시작                           | JSP 페이지 처리 완료                                         |
| request     | request      | 웹 브라우저부터 요청처리 시작                  | 웹 브라우저로 응답 완료                                      |
| session     | session      | 웹 브라우저의 첫번째 요청 처리 시작            | 세션 타이머가 만료되거나 명시적으로 세션을 소멸시킬 때 소멸됨 |
| application | application  | Tomcat의 구동과 함께 웹 어플리케이션의 첫 시작 | Tomcat서버의 종료                                            |

