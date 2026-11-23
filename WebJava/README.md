# 웹자바 연습문제 풀이

업로드한 PDF의 연습문제 **총 9문제**와 답안 코드를 GitHub에서 직접 확인할 수 있도록 옮겼습니다. 본문의 코드와 문제 내용은 원본 기준으로 유지했습니다.

## 목차

- [2장. JSP 프로그래밍 기초](#2장-jsp-프로그래밍-기초)
- [4장. 폼 태그를 이용한 값 입력](#4장-폼-태그를-이용한-값-입력)
- [6장. 데이터베이스를 이용한 프로그래밍](#6장-데이터베이스를-이용한-프로그래밍)
- [8장. 쿠키, 세션과 로그인](#8장-쿠키-세션과-로그인)
- [11장. EL과 JSTL](#11장-el과-jstl)
- [12장. 서블릿](#12장-서블릿)

---

## 2장. JSP 프로그래밍 기초

### 4번 문제

다음과 같은 출력을 만드는 JSP 프로그램을 작성하시오. 단 마지막 두 줄의 계산 결과(35, 250)는 소스 코드에 직접 써넣지 말고 프로그램이 계산하여 출력하도록 하되, 표현식 <%= %>을 사용하시오.

~~~text
     공백을 5개 찍고 이 문장이 시작됩니다.
이 문장은 중간에 공백이     5개 있습니다.
25 + 10 = 35
25 * 10 = 250
~~~

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>연습문제 2장 4번</title>
</head>
<body>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공백을 5개 찍고 이 문장이 시작됩니다.<br>
이 문장은 중간에 공백이&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5개 있습니다.<br>
25 + 10 = <%= 25 + 10 %><br>
25 * 10 = <%= 25 * 10 %><br>
</body>
</html>
```

---

## 4장. 폼 태그를 이용한 값 입력

### 1번 문제

한 학생의 국어, 영어, 수학 점수를 입력 받아 세 과목의 점수와, 총점, 평균을 출력하는 프로그램을 작성하시오.

학생의 점수를 입력 받는 폼은 예제 4-1.html을 그대로 사용한다. 넘겨받은 점수, 그리고 총점과 평균을 출력하는 프로그램은 예제 4-2.jsp를 수정해서 작성한다.

request.getParameter()로 얻어낸 입력 값은 문자열이다. 따라서 이렇게 얻어낸 문자열에, 정수는 Integer.parseInt(), 실수는 Float.parseFloat() 등의 메서드를 사용해서 숫자로 바꾸어 계산하여야 한다.

평균은 소수점 아래 2자리까지 출력한다. 다음과 같이 하면 소수점 3번째 자리에서 반올림되어 소수점 2번째 자리까지 숫자가 있는 문자열을 얻을 수 있다.

String.format("%.2f", 평균값)

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>성적 결과</title>
</head>
<body>
<%
   int kor = Integer.parseInt(request.getParameter("kor"));
   int eng = Integer.parseInt(request.getParameter("eng"));
   int math = Integer.parseInt(request.getParameter("math"));
   int total = kor + eng + math;
%>
국어 : <%=kor%><br>
영어 : <%=eng%><br>
수학 : <%=math%><br>
총점 : <%=total%><br>
평균 : <%=String.format("%.2f", total / 3.0)%><br>
</body>
</html>
```

### 2번 문제

원의 반지름을 입력 받아 원의 둘레와 면적을 출력하는 프로그램을 작성하시오. 반지름은 GET 방식으로 전달한다.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
  double r = Double.parseDouble(request.getParameter("r"));
%>
반지름 : <%=r%><br>
둘레 : <%=2 * Math.PI * r%><br>
넓이 : <%=Math.PI * r * r%><br>
</body>
</html>
```

---

## 6장. 데이터베이스를 이용한 프로그래밍

### 2번 문제

간단한 주소록을 위해 테이블을 생성하는 프로그램을 작성하시오. 이 테이블은 일련번호, 이름, 주소, 전화번호 필드를 가지도록 하고, executeUpdate() 메서드를 이용하여 작성하시오.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>DB 연결 테스트</title>
</head>
<body>
<%
Class.forName("org.mariadb.jdbc.Driver"); // 자바 커넥터 로드
try (Connection conn = DriverManager.getConnection("jdbc:mariadb://localhost:3308/jspdb","jsp", "1234");) {
  Statement stmt = conn.createStatement();
  String sql = "create table addrbook (" +
        "   num int primary key," +
        "  name varchar(20)," +
        "    addr varchar(80)," +
        "  phone varchar(20)" +
          ")";
stmt.executeUpdate(sql);
out.println("주소록 테이블 생성 성공!");
} catch (Exception e) {
  e.printStackTrace();
}
%>
</body>
</html>
```

### 3번 문제

앞에서 만든 주소록 테이블에 3명 이상의 샘플 데이터를 추가하는 프로그램을 작성하시오. 역시 executeUpdate() 메서드를 이용하여 작성하시오.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>DB 연결 테스트</title>
</head>
<body>
<%
Class.forName("org.mariadb.jdbc.Driver"); // 자바 커넥터 로드
try (Connection conn = DriverManager.getConnection("jdbc:mariadb://localhost:3308/jspdb","jsp", "1234");) {
  Statement stmt = conn.createStatement(); {
     stmt.executeUpdate("insert into addrbook values (1, '홍길동', '서울', '111-2222')");
     stmt.executeUpdate("insert into addrbook values (2, '이순신', '부산', '444-5555')");
     stmt.executeUpdate("insert into addrbook values (3, '강감찬', '대전', '888-8888')");
    out.println("쿼리 실행 성공!");
    }
} catch (Exception e) {
  e.printStackTrace();
}
%>
</body>
</html>
```

### 4번 문제

위 문제에서 만든 테이블의 데이터를 executeQuery() 메서드로 조회하는 프로그램을 작성하시오.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<style>
  table { width: 400px; text-align: center; }
  th    { background-color: cyan; }
</style>
</head>
<body>
<table border="1">
  <tr>
    <th>번호</th>
    <th>이름</th>
    <th>주소</th>
    <th>전화</th>
   </tr>
<%
Class.forName("org.mariadb.jdbc.Driver"); // 자바 커넥터 로드
try (
  Connection conn = DriverManager.getConnection(
     "jdbc:mariadb://localhost:3308/jspdb",
     "jsp",
    "1234"
   );
  Statement stmt = conn.createStatement();
  ResultSet rs = stmt.executeQuery("select * from addrbook");
) {
  while (rs.next()) {
%>
  <tr>
    <td><%= rs.getInt("num") %></td>
    <td><%= rs.getString("name") %></td>
    <td><%= rs.getString("addr") %></td>
    <td><%= rs.getString("phone") %></td>
   </tr>
<%
    }
} catch (Exception e) {
  e.printStackTrace();
}
%>
</table>
</body>
</html>
```

---

## 8장. 쿠키, 세션과 로그인

### 5번 문제

일반적으로 관리자 페이지에는 모든 회원들의 회원 정보 리스트를 볼 수 있는 기능이 있다. 회원 정보 리스트를 출력하는 프로그램을 작성해 보시오.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
  pageEncoding="UTF-8"%>
<%@ page import="java.sql.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원 정보 목록</title>
</head>
<body>
<h3>회원 정보 목록</h3>
<table border="1">
  <tr><th>아이디</th><th>이름</th></tr>
<%
  Class.forName("org.mariadb.jdbc.Driver");
   try (
    Connection conn = DriverManager.getConnection(
       "jdbc:mariadb://localhost:3308/jspdb", "jsp", "1234");
    Statement stmt = conn.createStatement();
     ResultSet rs = stmt.executeQuery("select id, name from member")
   ) {
     while (rs.next()) {
%>
  <tr>
    <td><%= rs.getString("id") %></td>
    <td><%= rs.getString("name") %></td>
   </tr>
<%
       }
    } catch (Exception e) {
    out.println("회원 목록 조회 실패");
     e.printStackTrace();
    }
%>
</table>
</body>
</html>
```

---

## 11장. EL과 JSTL

### 6번 문제

request의 속성인 age 값이 20 이상이면 “성인입니다.” 그렇지 않으면 “미성년입니다.”를 출력하는 JSTL 코드를 적어보시오.

**답:**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
  request.setAttribute("age", 21);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- JSTL choose -->
<!--     if - else 또는 if - else if - else 구조를 구현 -->
 <c:choose>
  <c:when test = "${age >= 20}">
   성인입니다.
   </c:when>
   <c:otherwise>
   미성년입니다.
   </c:otherwise>
 </c:choose>
</body>
</html>
```

---

## 12장. 서블릿

### 1번 문제

다음과 같은 URL이 주어졌을 때, 실행되어 name, kor, eng, math 값을 화면에 출력하는 서블릿을 작성하여 보시오.

http://localhost:8080/ch12/score?name=Hong&kor=78&eng=85&math=90

request.getParameter("name")

**답:**

```java
package com.servlet.exam;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;
@WebServlet("/Score")
public class Score extends HttpServlet {
  private static final long serialVersionUID = 1L;
  public Score() {
     super();
    }
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,
  IOException {
     response.setContentType("text/html; charset=utf-8");
     PrintWriter out = response.getWriter();
      out.println("<html>");
     out.println("<head>");
      out.println("  <meta charset=utf-8>");
     out.println("</head>");
     out.println("<body>");
     out.println("이름 :" + request.getParameter("name") + "<br>");
     out.println("국어 :" + request.getParameter("kor") + "<br>");
     out.println("영어 :" + request.getParameter("eng") + "<br>");
     out.println("수학 :" + request.getParameter("math") + "<br>");
     out.println("</body>");
      out.println("</html>");
    }
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException,
  IOException {
    doGet(request, response);
    }
}
```

> **참고:** 12장 1번의 원본 답안은 `@WebServlet("/Score")`로 작성되어 있으나, 문제 URL에는 `/score`가 사용되어 대소문자가 다릅니다. 원본 코드의 표기는 그대로 보존했습니다.
