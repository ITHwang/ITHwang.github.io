---
layout: single
title: "서블릿 API 정리"
toc: true
toc_sticky: true
tags:
  - java
  - servlet
categories:
  - Back-end
---

## Servlet API 관계도

---

[깝돌이 님의 글](https://ggapdol2.tistory.com/104)을 참고했습니다. 해당 글 내 관계도를 통해 정리하겠습니다.

## 머릿속 정리

---

### Servlet과 ServletConfig

1. Servlet의 `init()`으로 **ServletConfig**를 초기화합니다.<u>(Servlet당 하나의 ServletConfig를 가집니다.)</u>
2. Servlet의 `getServletConfig()`으로 해당 **ServletConfig**를 가져옵니다.
3. 따라서 두 인터페이스는 **연관 관계**(Association)입니다.

### ServletConfig와 ServletContext

1. ServletConfig의 `getServletContext()`로 **ServletContext** 객체를 가져옵니다.<u>(컨텍스트당 하나의 ServletContext를 가집니다.)</u>
2. 따라서 두 인터페이스 또한 **연관 관계**(Association)입니다.

### GenericServlet

1. **Servlet**을 상속 받아 일반 서블릿 기능을 수행합니다.
2. **ServletConfig**를 상속 받아 WAS 관련 기능을 수행합니다.
3. 두 인터페이스 <u>ServletRequest, ServletResponse</u>를 매개변수로 받는 추상 메서드 `service()`가 있습니다.

### HttpServlet

1. **GenericServlet**을 상속 받아 <u>HTTP 프로토콜에 종속적인 서블릿 기능</u>을 수행합니다.
2. **GenericServlet**의 추상 메서드 `service()`을 두 인터페이스 <u>HttpServletRequest, HttpServletResponse</u>를 매개변수로 받아 오버라이딩합니다.<u>(각각 ServletRequest와 ServletResponse를 상속 받기 때문에 오버라이딩이 가능합니다.)</u>
3. 오버라이딩된 `service()`는 doXXX에 해당하는 **service method**에 클라이언트 요청을 전송(dispatch)합니다.

## Reference

---

- [servlet API 관계도](https://ggapdol2.tistory.com/104)
