---
layout: single
title: "서블릿-데이터베이스 연동하기"
toc: true
toc_sticky: true
tags:
  - java
  - servlet
  - DB
  - tomcat

categories:
  - Back-end
---

## 데이터베이스 연결

---

자바 서블릿 클래스와 데이터베이스의 연결을 위해 각 DB에서 JDBC 드라이버를 제공합니다.
오라클 DB에서는 `ojdbc`라는 jar 파일을 제공하고 있습니다.
이를 <u>{프로젝트}\src\main\webapp\WEB-INF\lib</u>에 두면 서버가 드라이버를 찾을 수 있습니다.
저는 `ojdbc6.jar`으로 실습했습니다.

연결 과정은 다음과 같습니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210603-servlet-db-connectivity-1.png?raw=true"></p>

1. **톰캣**(웹 애플리케이션 서버)를 실행한 후, 드라이버를 찾아 로드합니다.
2. `DriverManager` 클래스를 활용하여 `Connection` 객체를 생성합니다.
3. Connecion 객체로 `Statement` 객체를 생성합니다.
4. Statement 객체가 전달 받은 쿼리를 실행함으로써 `ResultSet` 객체를 반환합니다.

코드로 보면 다음과 같습니다.

- Code

  ```java
  private static final String driver = "oracle.jdbc.driver.OracleDriver"; // 드라이버 이름
  private static final String url = "jdbc:oracle:thin:@localhost:1521:XE"; // 접속변수
  private static final String user = "SERVLET"; // 사용자 이름
  private static final String pwd = "12345"; // 사용자 비밀번호
  private Connection con;
  private Statement stmt;

  private void connDB() {
      try {
          Class.forName(driver);
          System.out.println("Oracle 드라이버 로딩 성공");

          con = DriverManager.getConnection(url, user, pwd);
          System.out.println("connection 생성 성공");

          stmt = con.createStatement();
          System.out.println("Statement 생성 성공");
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
  ```

## 서블릿으로 데이터베이스 조회하기

---

아래와 같은 회원 정보 테이블 `t_member`가 있다고 합시다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210603-servlet-db-connectivity-2.png?raw=true"></p>

각 칼럼을 필드로 가지는 클래스 `MemberVO`를 만듭니다.

- Code

  ```java
  public class MemberVO {
      private String id;
      private String pwd;
      private String name;
      private String email;
      private Date joinDate;

      // ...
      // getters and setters
  ```

이 클래스를 활용하여 모든 회원 정보를 조회하는 함수를 구현할 수 있습니다.

- Code

  ```java
  public List<MemberVO> listMembers() {
      List<MemberVO> list = new ArrayList<MemberVO>();

      try {
          connDB(); //statement 객체인 stmt에 메모리 할당
          String query = "select * from t_member ";
          ResultSet rs = stmt.executeQuery(query); //쿼리문 실행
          while (rs.next()) {
              //조회한 레코드에서 각 칼럼 값 받아오기
              String id = rs.getString("id");
              String pwd = rs.getString("pwd");
              String name = rs.getString("name");
              String email = rs.getString("email");
              Date joinDate = rs.getDate("joinDate");

              //MemberVO 객체에 담아 리스트에 추가
              MemberVO vo = new MemberVO();
              vo.setId(id);
              vo.setPwd(pwd);
              vo.setName(name);
              vo.setEmail(email);
              vo.setJoinDate(joinDate);
              list.add(vo);
          }
          rs.close();
          stmt.close();
          con.close();
      } catch (Exception e) {
          e.printStackTrace();
      }

      return list; //리스트 반환
  }
  ```

## 개선1 - PreparedStatement

---

`Statement` 인터페이스를 이용하여 DB와 연동할 경우, 연동할 때마다 DBMS에서 쿼리를 컴파일하므로 속도가 느립니다.
반면 `Statement`를 상속 받은 `PreparedStatement` 인터페이스는 **컴파일된 쿼리**를 DBMS에 전달하므로 성능을 향상시킵니다.
또한 물음표를 이용한 포맷팅으로 `Statement`보다 간단하게 쿼리를 작성할 수 있습니다.

- Code

  ```java
  public List<MemberVO> listMembers() {
      List<MemberVO> list = new ArrayList<MemberVO>();

      try {
          connDB(); //(수정)Connection인 con에 Connection 객체 할당
          String query = "select * from t_member ";
          System.out.println("prepareStatement: " + query);

          //PreparedStatement인 pstmt에 sql문을 전달하여 객체 생성
          pstmt = con.prepareStatement(query);
          //쿼리문 실행
          ResultSet rs = pstmt.executeQuery();

          while (rs.next()) {
              //생략
      } catch (Exception e) {
          e.printStackTrace();
      }

      return list; //리스트 반환
  }
  ```

## 개선2 - ConnectionPoll

---

데이터베이스 연결은 시간이 많이 듭니다.
따라서 작업할 때마다 DB와의 연결을 시도하는 것은 비효율적입니다.
이를 보완하는 **커넥션풀(ConnectionPool)**은 웹 애플리케이션이 실행될 때 <u>연동할 DB와의 연결을 미리 설정하고 유지</u>하여, 작업할 때마다 빠르게 DB와 연동시켜줍니다.

<p style="text-align: center;"><img src="https://github.com/ITHwang/ITHwang.github.io/blob/master/_images/20210603-servlet-db-connectivity-3.png?raw=true"></p>

톰캣이 실행될 때 `context.xml`에 설정된 DB 정보로 미리 연결되고 `ConnectionPool` 객체가 생성됩니다.
이후 애플리케이션이 작업할 때마다 connectionPool 객체의 메서드로 DB와 빠르게 연동할 수 있습니다.

톰캣이 제공하는 `ConnectionPool` 객체를 이용하기 위해서는 두 가지 설정이 필요합니다.

1. `tomcat-dbcp.jar` 파일을 /WEB-INF/lib 폴더에 설치합니다.
2. CATALINA_HOME/context.xml에 연결할 DB 정보를 **Resource** 태그 안에 다음과 같이 삽입합니다.
   ```
   name="jdbc/oracle"
   auth="Container"
   type="javax.sql.DataSource"
   driverClassName="oracle.jdbc.OracleDriver"
   url="jdbc:oracle:thin:@localhost:1521:XE"
   username="scott"
   password="tiger"
   maxActive="50"
   maxWait="-1"
   ```

실제 구현할 때에는 `javax.sql.DataSource` 클래스를 이용하여 `ConnectionPool` 객체를 구현할 수 있습니다.

- Code

  ```java
  import java.sql.Connection;
  import java.sql.Date;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.util.ArrayList;
  import java.util.List;
  import javax.naming.Context;
  import javax.naming.InitialContext;
  import javax.sql.DataSource;

  public class MemberDAO {
      private Connection con;
      private PreparedStatement pstmt;
      private DataSource dataFactory;

      public MemberDAO() {
          try {
              //JNDI(Java Naming and Directory Interface)에 접근하기 위해 기본 경로 지정
              Context ctx = new InitialContext();
              Context envContext = (Context) ctx.lookup("java:/comp/env");

              //context.xml에서 설정한 name 값을 이용하여 ConnectionPool 객체를 불러온다.
              dataFactory = (DataSource) envContext.lookup("jdbc/oracle");
          } catch (Exception e) {
              e.printStackTrace();
          }
      }

      public List<MemberVO> listMembers() 략
          List<MemberVO> list = new ArrayList<MemberVO>();

          try {
              con = dataFactory.getConnection(); //ConnectionPool 객체를 이용하여 DB 연결
              String query = "select * from t_member ";
              System.out.println("prepareStatement: " + query);
              pstmt = con.prepareStatement(query);
              ResultSet rs = pstmt.executeQuery();

              while (rs.next()) {
                  //생략
          } catch (Exception e) {
              e.printStackTrace();
          }

          return list;
      }
  }
  ```

## Conclusion

---

자바 서블릿이 오라클 DB에서 데이터를 어떻게 가져오는지에 대해 살펴보았습니다.
더불어 sql에게 이미 컴파일된 쿼리를 전달하는 `PreparedStatement`와 DB와의 연동을 유지시켜주는 `ConnectionPool`을 이용하여 성능을 높였습니다.
감사합니다!!

## Reference

---

- 이병승, 『자바 웹을 다루는 기술』, 길벗, 2019, 6~7장
