# 웹자바프로그래밍입문 중간고사 내용 정리

> **범위:** 업로드한 **1~9장 전체**. 공부하기 대화에서 요청한 방식대로 문제풀이 없이 학습 개념만 정리했습니다.

## 1. 장별 핵심 내용

| 장 | 핵심 개념 |
|---|---|
| **1장 JSP 실행 원리** | HTML은 브라우저에서 표시, **JSP**는 **Tomcat**에서 서버 처리 후 HTML 응답 생성. JDK·Tomcat·포트·URL |
| **2장 JSP 기초** | 지시자 `<%@ ... %>`, 스크립틀릿 `<% ... %>`, 표현식 `<%= ... %>`, `out.println()`, JSP 주석 |
| **3장 Eclipse** | Workspace(작업 공간), Dynamic Web Project, Tomcat 서버 등록, UTF-8, WAR 배포 |
| **4장 폼 입력** | `form`의 **action / method / name**, GET·POST 차이, `request.getParameter()`·`getParameterValues()` |
| **5장 MariaDB** | DBMS·테이블·레코드·필드, **CREATE / INSERT / SELECT / UPDATE / DELETE**, HeidiSQL |
| **6장 JDBC** | `Connection`, `Statement`, `ResultSet`. **`executeUpdate()`**(변경) vs **`executeQuery()`**(조회), `rs.next()` |
| **7장 파일 업로드** | `multipart/form-data`, `request.getPart()`, `Part.getSubmittedFileName()`, `Part.write()`, 파일과 DB 메타데이터 |
| **8장 쿠키·세션** | 쿠키는 브라우저, 세션 상태는 서버 측 관리. `session.setAttribute()`, `getAttribute()`, `removeAttribute()`; 로그인·회원가입 |
| **9장 게시판** | **CRUD**: 목록 `list.jsp`, 입력 `write.jsp`, 등록 `insert.jsp`, 조회 `view.jsp`, 수정·삭제 `update.jsp`·`delete.jsp` |

## 2. 자주 연결되는 처리 흐름
- **폼 처리** : 입력 폼 → GET/POST 요청 → `request.getParameter("name")` → 결과 출력.
- **데이터베이스 조회** : JDBC 연결 → SQL 실행 → `ResultSet` → `rs.next()`로 행 읽기 → 자원 정리.
- **데이터 변경** : SQL을 `executeUpdate()`로 실행하고 결과에 따라 화면 처리.
- **로그인** : 아이디·비밀번호 입력 → DB 확인 → 로그인 성공 시 세션 속성 등록 → 로그아웃 시 제거.
- **파일 업로드** : `enctype="multipart/form-data"` → `Part`로 파일 수신 → 파일 저장 → DB에 파일 정보 기록.
- **게시판** : 글 번호를 이용한 상세 조회·수정·삭제, 목록은 여러 행을 반복 출력.

## 3. 핵심 메서드 한눈에 보기
- `out.println()` : JSP 응답에 내용 출력. HTML 화면 줄바꿈은 `<br>` 등 마크업으로 처리.
- `request.getParameter()` : 단일 입력값 / `getParameterValues()` : 같은 이름의 여러 입력값.
- `executeUpdate()` : INSERT·UPDATE·DELETE·DDL / `executeQuery()` : SELECT 결과 조회.
- `session.setAttribute()` : 세션 값 저장 / `response.sendRedirect()` : 다른 URL로 이동 응답.
- **JSP 주석** `<%-- ... --%>`는 응답에서 제거되고 HTML 주석은 소스에 남을 수 있음.

## 4. 핵심 정리
- 학습 대화에서 복습 비중이 컸던 순서는 **4·6장 → 8·9장 → 2·5장 → 7장 → 1·3장**.
- JSP 요청·응답, 폼 입력, JDBC 데이터 처리, 세션, 게시판 **전체 연결 흐름** 이해.
- 자세한 내용: [1~9장 TIL 목차](../README.md#웹자바프로그래밍입문)
