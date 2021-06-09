---
layout: single
title: "서블릿 API - ServletContext & ServletConfig"
toc: true
toc_sticky: true
tags:
  - java
  - servlet

categories:
  - Back-end
---

## Background

---

한 애플리케이션(컨텍스트) 내에 있는 서블릿들과 JSP들은 데이터를 공유하거나 전달합니다.

예를 들어 서블릿에서 다른 서블릿 또는 JSP로 포워딩할 때 데이터도 같이 보낸다면, 주소와 물음표 뒤에 값과 이름이 결합된 형태인 GET 방식을 활용할 수 있습니다.
그러나 GET은 전송 데이터에 제한이 있으므로 대량의 데이터를 공유하거나 전달하고 싶을 때는 바인딩 기능을 이용합니다.

여러 방법을 통해 바인딩을 구현할 수 있는데, 그중 하나가 `ServletContext` 및 `ServletConfig` 인터페이스를 이용한 것입니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210608-servletcontext-servletconfig.png?raw=true"></p>

톰캣 컨테이너 실행 시 각 컨텍스트마다 한 개의 ServletContext 객체가 생성되며, 각 서블릿마다 한 개의 ServletConfig가 생성됩니다.(GenericServlet 클래스가 ServletConfig 인터페이스를 구현합니다.)
이후 ServletConfig는 서블릿이 소멸할 때 같이 소멸되고, ServletContext는 컨테이너가 종료될 때 같이 종료됩니다.

한 서블릿이 ServletContext 객체에 데이터를 바인딩한다면, 같은 컨텍스트에 있는 다른 서블릿들은 굳이 포워딩을 받지 않아도 ServetConfig의 메서드로 ServletContext를 불러 데이터에 접근할 수 있습니다.

## Code

---

SetServletContext 서블릿이 ServletContext 객체에 이름과 나이를 바인딩하고, GetServletContext 서블릿이 이를 가져오는 예시 코드입니다.

- SetServletContext.java

  ```java
  package sec05.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.util.ArrayList;
  import java.util.List;

  import javax.servlet.ServletContext;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  @WebServlet("/cset")
  public class SetServletContext extends HttpServlet {

      protected void doGet(HttpServletRequest request, HttpServletResponse response)
              throws ServletException, IOException {
          response.setContentType("text/html;charset=utf-8");
          PrintWriter out = response.getWriter();

          //ServletContext 가져오기(ServletConfig의 메서드)
          ServletContext context = getServletContext();

          List member = new ArrayList(); //데이터 생성
          member.add("이순신");
          member.add(30);

          context.setAttribute("member", member); //데이터 바인딩
          out.print("<html><body>");
          out.print("이순신과 30 설정");
          out.print("</body></html>");
      }
  }

  /*
  Output:

  이순신과 30 설정
  */
  ```

- GetServletContext.java

  ```java
  package sec05.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import java.util.ArrayList;
  import java.util.List;

  import javax.servlet.ServletContext;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;

  @WebServlet("/cget")
  public class GetServletContext extends HttpServlet {

      protected void doGet(HttpServletRequest request, HttpServletResponse response)
              throws ServletException, IOException {
          response.setContentType("text/html;charset=utf-8");
          PrintWriter out = response.getWriter();

          //ServletContext 가져오기(ServletConfig의 메서드)
          ServletContext context = getServletContext();
          List member = (ArrayList) context.getAttribute("member"); //바인딩된 데이터 접근

          String name = (String) member.get(0);
          int age = (Integer) member.get(1);

          out.print("<html><body>");
          out.print(name + "<br>");
          out.print(age + "<br>");
          out.print("</body></html>");
      }
  }

  /*
  Output:

  이순신
  30
  */
  ```

## 이외에도

---

HttpServletRequest 객체에 데이터를 바인딩하고 포워딩하는 전달 방법도 있습니다.
다만 요청 정보 유지를 위해 클라이언트를 거치지 않는 dispatch 포워딩을 사용해야 합니다.

## Reference

---

- 이병승, 『자바 웹을 다루는 기술』, 길벗, 2019, 8장
