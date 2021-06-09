---
layout: single
title: "세션 트래킹 - Cookie & Session"
toc: true
toc_sticky: true
tags:
  - java
  - servlet
  - cookie
  - session
categories:
  - Back-end
---

## 세션 트래킹

---

HTTP 프로토콜은 서버-클라이언트 통신 시 **stateless 방식**으로 통신합니다.
즉, 각 웹 페이지가 가지고 있는 상태나 정보를 다른 페이지들과 공유할 수 없습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210609-session-tracking-1.png?raw=true"></p>

일반 유저들은 웹 페이지들이 로그인 창에 입력한 ID 및 PW 정보를 모두 알고 있을 거라 생각하지만, 사실 그렇지 않습니다.
따라서 서블릿끼리 상태나 정보를 공유하기 위해서 웹 페이지 연결 기능인 **세션 트래킹**(Session Tracking)을 이용해야 합니다.

웹 페이지 간 연동하기 위해서는 `<input>` 태그에 hidden type을 부여하여 전송하거나, URL과 물음표 뒤에 정보를 붙여서 다른 페이지로 전송하는 **URL Rewriting 방식**을 이용하곤 합니다.
그러나 이 두 가지는 단점들이 있습니다.

1. 로그인 상태를 확인하기 위해선 무조건 전송을 받아야 합니다.
2. GET 방식으로 전송 받으므로 보안상으로 좋지 않습니다.
3. GET 방식으로 전송 받으므로 데이터 용량에 한계가 있습니다.

따라서 정보들을 <u>아예 클라이언트 PC나 서버 메모리에 저장해두고 공유하는 방법</u>이 고안되었는데, 바로 **쿠키**(Cookie)와 **세션**(Session)입니다.

## 쿠키

---

**쿠키**(Cookie)는 웹 페이지 사이의 공유 정보를 **클라이언트 PC**에 저장하는 연동 방법입니다.
생성 위치에 따라 **Persistence** 쿠키와 **Session** 쿠키로 나뉘는데 표로 보면 다음과 같습니다.

<style>
  table{text-align : center; border-collapse : collapse; width : 400px;}
  tr, td{border : 3px solid;}
  thead{font-weight : bolder;}
</style>
<table>
    <thead>
    <tr><td>속성</td><td>Persistence 쿠키</td><td>Session 쿠키</td></tr>
    </thead>
    <tbody>
    <tr><td>저장 위치</td><td colspan=2>클라이언트 PC</td></tr>
     <tr><td>저장 용량</td><td colspan=2>최대 4kb</td></tr>
    <tr><td>보안 강도</td><td colspan=2>약함</td></tr>
    <tr><td>생성 방법</td><td>파일로 생성</td><td>브라우저 메모리에 생성</td></tr>
    <tr><td>종료 시기</td><td>쿠키를 삭제하거나 쿠키 설정 값이 종료된 경우</td><td>브라우저를 종료한 경우</td></tr>
    </tbody>
</table>
<br>

### 실행 과정

- 최초 접속

  1. 서버: 쿠키 생성
  2. 서버: 쿠키를 브라우저에 전송
  3. 브라우저: 받은 쿠키를 파일 또는 브라우저 메모리에 저장

- 재접속
  1. 서버: 브라우저에게 쿠키 전송 요청
  2. 브라우저: 서버에게 쿠키 전송
  3. 서버: 쿠키 정보를 이용해 작업 수행

### Code

1. SetCookieValue 서블릿이 문자열을 쿠키에 생성하여 클라이언트에게 보냅니다.
2. 클라이언트는 파일 또는 브라우저 메모리에 저장합니다.
3. GetCookieValue 서블릿이 클라이언트에게 Cookie를 받아 출력합니다.

- SetCookieValue.java

  ```java
  package sec02.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.net.URLEncoder;
  import java.util.Date;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.Cookie;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  @WebServlet("/set")
  public class SetCookieValue extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
      response.setContentType("text/html;charset=utf-8");
      PrintWriter out = response.getWriter();

      Date d = new Date(); //현재 시간
      //한글 문자열 인코딩 후 cookieTest라는 쿠키에 값으로 저장
      Cookie c = new Cookie("cookieTest", URLEncoder.encode("JSP프로그래밍입니다.", "utf-8"));

      //유효기간 설정: 양수일 경우 Persistence 쿠키, 음수이거나 메서드를 사용하지 않으면 Session 쿠키로 저장
      //c.setMaxAge(24*60*60);
      c.setMaxAge(-1);

      response.addCookie(c); //브라우저에게 쿠키 전송
      out.println("현재시간 : " + d + "<br>");
      out.println("문자열을 Cookie에 저장합니다.");
    }
  }

  /*
  Output:

  현재시간 : ...
  문자열을 Cookie에 저장합니다.
  */
  ```

- GetCookieValue.java

  ```java
  package sec02.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.net.URLDecoder;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.Cookie;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  @WebServlet("/get")
  public class GetCookieValue extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
      response.setContentType("text/html;charset=utf-8");
      PrintWriter out = response.getWriter();

      Cookie[] allValues = request.getCookies(); //브라우저에게 쿠키 요청

      for (int i = 0; i < allValues.length; i++) {
        if (allValues[i].getName().equals("cookieTest")) { //cookieTest 쿠키 찾으면
          out.println("<h2>Cookie 값 가져오기 : " + URLDecoder.decode(allValues[i].getValue(), "utf-8") + "</h2>");
        }
      }
    }
  }

  /*
  Output:

  Cookie 값 가져오기 : JSP프로그래밍입니다.
  */
  ```

## 세션

---

쿠키는 클라이언트 PC에 저장되므로 <u>외부에 쉽게 노출된다는 단점</u>이 있습니다.
반면 **세션**(Session)은 **서버 메모리**에 저장되어 비교적 보안이 강합니다.
따라서 로그인 정보와 같이 <u>보안이 요구되는 정보들은 대부분 세션을 이용</u>합니다.

- 특징

  1. 서버 메모리에 저장되어 보안에 유리합니다.
  2. 세션을 참조할 수 있는 **세션 id**를 브라우저에 전송하며, 브라우저는 <u>세션 쿠키에 저장</u>합니다.(브라우저당 한 개)
  3. 세션은 유효 시간(디폴트 30분)을 가집니다.(`CATALINA_HOME/web.xml` 내 `<session-config><session-timeout>` 태그 안에서 설정 가능)
  4. 로그인 상태 유지나 장바구니 담기 등에 주로 사용됩니다.

### 실행 과정

- 최초 접속

  1. 서버: 세션 객체 생성
  2. 서버: 세션 객체를 참조할 세션 id를 브라우저에 전송
  3. 브라우저: 서버로부터 받은 세션 id를 세션 쿠키에 저장

- 재접속
  1. 브라우저: 서버에게 세션 id 전송
  2. 서버: 받은 세션 id로 세션 객체에 접근하여 작업 수행

### Code

세션 API를 통해 세션 객체를 생성하고 관련 정보를 출력해보겠습니다.

- SessionTest.java

  ```java
  package sec03.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.util.Date;

  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import javax.servlet.http.HttpSession;

  @WebServlet("/sess")
  public class SessionTest extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
      response.setContentType("text/html;charset=utf-8");
      PrintWriter out = response.getWriter();

      //최초 요청 시 세션 객체 새로 생성, 재요청 시 기존 세션 반환
      HttpSession session = request.getSession();

      out.println("세션 아이디: " + session.getId() + "<br>");
      out.println("최초 세션 생성 시각: " + new Date(session.getCreationTime()) + "<br>");
      out.println("최근 세션 접근 시각: " + new Date(session.getLastAccessedTime()) + "<br>");
      out.println("세션 유효 시간: " + session.getMaxInactiveInterval() + "<br>");

      if (session.isNew()) {
        out.print("새 세션이 만들어졌습니다.");
      }
    }
  }

  /*
  Output:

  세션 아이디: ...
  최초 세션 생성 시각: ...
  최근 세션 접근 시각: ...
  세션 유효 시간: 1800 //톰캣에 설정된 기본 유효 시간
  새 세션이 만들어졌습니다. //재접속하면 출력되지 않음
  */
  ```

## Reference

---

- 이병승, 『자바 웹을 다루는 기술』, 길벗, 2019, 9장
