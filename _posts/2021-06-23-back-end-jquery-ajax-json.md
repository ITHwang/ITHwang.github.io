---
layout: single
title: "jQuery ajax로 json 주고받기"
toc: true
toc_sticky: true
tags:
  - jquery
  - ajax
  - json
categories:
  - Back-end
---

## Ajax란?

---

**Ajax**란 <u>Asynchronous Javascript + XML</u>의 의미로, 자바스크립트를 이용해 클라이언트와 서버가 XML이나 JSON 데이터를 주고받는 **비동기 통신 기술**입니다.

서버가 응답할 때까지 클라이언트가 기다리는 순차적인 방식을 **동기적 통신**이라고 한다면, <u>클라이언트가 서버 응답을 기다리지 않고, 서버 작업과 동시에 자신의 작업을 수행하는 방식</u>을 **비동기 통신**이라고 볼 수 있습니다.
즉, [생활코딩 이고잉님](https://opentutorials.org/course/53/49)에 따르면 **비동기**란 여러가지 일이 동시에 발생한다는 뜻입니다.

이전에는 서버의 응답을 새로운 페이지에 받아 브라우저에 출력해야 했지만, ajax 덕분에 <u>페이지 이동 없이 데이터 처리</u>를 할 수 있게 되었습니다.
더불어 서버에서 응답 받을 때마다 <u>브라우저가 일일이 페이지를 구성하지 않아도 되어</u> 성능도 향상되었습니다.

보통 ajax를 연동할 때 XML 형식으로 데이터를 주고받으나, 내부적으로 여러 단계의 과정을 거치게 되므로 자원이 열악한 모바일 환경에서는 보다 형식이 단순한 **JSON**으로 데이터를 주고받습니다.

jQuery에서 지원하는 ajax로 서버와 클라이언트를 연동하여 json 데이터를 주고받는 실습을 해보겠습니다.

## 회원 정보 수신하기

---

클라이언트(jsp)가 회원 정보를 요청하면, 서버(servlet)가 두 개의 회원 정보를 생성한 후 브라우저로 전송하는 실습입니다.

- json6.jsp

  ```html
  <%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8" isELIgnored="false" %>
  <!--isELIgnored: 표현언어 사용하기-->
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <!--JSTL core 라이브러리 사용하기-->

  <c:set var="contextPath" value="${pageContext.request.contextPath}" />
  <!--컨텍스트 경로 담은 변수 생성-->
  <!DOCTYPE html>
  <html>
    <head>
      <title>JSON 테스트</title>
      <script src="http://code.jquery.com/jquery-latest.min.js"></script>
      <script>
        $(function () {
          //'회원 정보 수신하기' 버튼 클릭하면
          $("#checkJson").click(function () {
            //ajax 연동
            $.ajax({
              type: "post", //post로 송수신
              async: true, //true가 디폴트, false일 경우 동기 방식으로 송수신
              url: "${contextPath}/json2", //요청 보낼 서버 경로

              //송수신에 성공할 경우 서버로부터 받은 데이터를 첫 번째 인자에 담는다.
              success: function (data, textStatus) {
                var jsonInfo = JSON.parse(data); //문자열로 받은 배열 데이터를 json 형식으로 파싱
                var memberInfo = "회원 정보<br>";
                memberInfo += "=======<br>";

                //차례대로 데이터 접근하여 html 생성하기
                for (var i in jsonInfo.members) {
                  memberInfo += "이름: " + jsonInfo.members[i].name + "<br>";
                  memberInfo += "나이: " + jsonInfo.members[i].age + "<br>";
                  memberInfo += "성별: " + jsonInfo.members[i].gender + "<br>";
                  memberInfo +=
                    "별명: " + jsonInfo.members[i].nickname + "<br><br><br>";
                }
                $("#output").html(memberInfo); //<div id="output"></div> 안에 담기
              },

              //송수신에 실패할 경우
              error: function (data, textStatus) {
                alert("에러가 발생했습니다.");
                ㅣ;
              },

              //ajax 작업이 완료된 경우
              complete: function (data, textStatus) {},
            });
          });
        });
      </script>
    </head>
    <body>
      <a id="checkJson" style="cursor:pointer">회원 정보 수신하기</a
      ><br /><br />
      <div id="output"></div>
    </body>
  </html>
  ```

- JsonServlet2.java

  ```java
  package sec03.ex01;

  import java.io.IOException;
  import java.io.PrintWriter;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import org.json.simple.JSONArray;
  import org.json.simple.JSONObject;

  @WebServlet("/json2")
  public class JsonServlet2 extends HttpServlet {
  	protected void doGet(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		doHandle(request, response);
  	}

  	protected void doPost(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		doHandle(request, response);
  	}

  	private void doHandle(HttpServletRequest request, HttpServletResponse response)
  			throws ServletException, IOException {
  		request.setCharacterEncoding("utf-8");
  		response.setContentType("text/html; charset=utf-8");
  		PrintWriter writer = response.getWriter();

  		JSONObject totalObject = new JSONObject(); //배열 담을 객체 생성
  		JSONArray membersArray = new JSONArray(); //회원 객체 담을 배열 생성

  		JSONObject memberInfo = new JSONObject(); //첫 번째 회원 객체 생성
  		memberInfo.put("name", "박지성");
  		memberInfo.put("age", "25");
  		memberInfo.put("gender", "남자");
  		memberInfo.put("nickname", "날센돌이");
  		membersArray.add(memberInfo); //배열에 추가

  		memberInfo = new JSONObject(); //두 번째 회원 객체 생성
  		memberInfo.put("name", "김연아");
  		memberInfo.put("age", "21");
  		memberInfo.put("gender", "여자");
  		memberInfo.put("nickname", "칼치");
  		membersArray.add(memberInfo); //배열에 추가

  		totalObject.put("members", membersArray); //클라이언트에게 보낼 객체에 배열 담기

  		String jsonInfo = totalObject.toJSONString(); //문자열 변환
  		System.out.println(jsonInfo); //콘솔에 출력
  		writer.print(jsonInfo); //클라이언트에게 전송
  	}
  }
  ```

## Reference

---

- [생활코딩 Ajax](https://opentutorials.org/course/53/49)
- 이병승, 『자바 웹을 다루는 기술』, 길벗, 2019, 16장
