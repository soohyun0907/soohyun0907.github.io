---
title: "백앤드 개념1 - Servlet"
excerpt: "servlet"

categories:
 - Back-End
tags:
 - Web
 - Servlet
layout: single
---

# Web Architecture

<img width="793" alt="web_architecture" src="https://user-images.githubusercontent.com/33771279/79084603-99005880-7d6f-11ea-9fb2-9e504c3a3a30.png">

- Web Browser : Markup, HTML, XML, CSS, Script(JS, Flash)
  - HTTP stateless : connect -> request -> response -> disconnect
- script언어들은 데이터 베이스에 직접적 접근이 어렵다.
- 웹에서 페이지를 이동하는 방식 2가지 : get, post, put, delete 등
- 서버 : Web Server, Web Application Server(WAS)
  - Web Server : Apache
  - WAS : Servlet, JSP

# Servlet

- Server Side Applet의 약어
- 웹 서버, 즉 웹 컨테이너에서 수행되는 JAVA 클래스

## 특징

- 서버의 응용 프로그램을 구현하는 기술로서 서버 프로토콜 종류에 관계없이 FTP, SMTP, HTTP 등 여러가지 애플리케이션 기반의 응용 프로그램을 개발할 수 있다.
- 서블릿은 클라이언트의 요청에 대하여 서블릿 컨테이너에 의해 독립된 스레드 기반으로 서비스가 되는 기술로서 다중 스레드 서비스가 기본적으로 제공된다. 그러므로 프로세스 기반의 서비스인 CGI에 비해 수행속도가 빠르다.
- 서블릿 컨테이너는 클라이언트에서 전송되는 서블릿 요청과 응답에 대한 처리를 담당한다.

## Servlet API

- javax.servlet package
- javax.servlet.http package

## Servlet LifeCycle

| method    | description                                                  |
| --------- | ------------------------------------------------------------ |
| init()    | 서블릿이 메모리에 로드 될 때 한번 호출.<br />코드 수정으로 인해 다시 로드 되면 다시 호출. |
| doGet()   | GET방식으로 데이터 전송 시 호출.                             |
| doPost()  | POST방식으로 데이터 전송 시 호출.                            |
| service() | 모든 요청은 service()를 통해서                               |
| destory() | 서블릿이 가비지 컬렉션(gabage collection : gc)가 되기 전에 마지막으로 수행해야할 작업이 있다면 destory()에 정의 |

- 서블릿에서는 ServletRequest 객체를 이용하여 요청을 처리하고 응답에 해당하는 결과 페이지를 ServletResponse 객체에서 얻은 PrintWriter 객체를 통해서 출력한다.

## Servlet에서의 parameter 처리

|      | GET                                                          | POST                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 특징 | 전송되는 데이터가 URL뒤에 QueryString으로 전달. <br />입력 값이 적은 경우나 데이터가 노출이 되도 문제가 없을 경우 사용. | URL과 별도로 전송.<br />HTTP header 뒤 body에 입력 스트림 데이터로 전달. |
| 장점 | 간단한 데이터를 빠르게 전송.<br />form tag뿐만 아니라 직접 URL에 입력하여 전송 가능. | 데이터의 제한이 없다.<br />최소한의 보안 유지 효과를 볼 수 있다. |
| 단점 | 데이터 양에 제한이 있다.<br />(location bar(URL+parameters))를 통해 전송할 수 있는 데이터의 사이즈는 2KB(2048 byte)로 제한된다. | 전달 데이터의 양이 같을 경우 GET방식보다 느리다. (전송 패킷을 body에 데이터를 구성해야 하므로) |

- 로직적으로 두 방식은 같지만 내부적인 데이터 처리 방식이 다르다.
- curl이 설치된 폴더로 이동 후 아래 명령어 입력

> curl -X POST localhost:8080/WebBoard/GetPostServlet
>
> curl -X GET localhost:8080/WebBoard/GetPostServlet

### GetPostServlet.java

```java


import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/GetPostServlet")
public class GetPostServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("doGet() is called");
		System.out.println("doGet() is called");
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("doPost() is called");
		System.out.println("doPost() is called");
	}

}
```

