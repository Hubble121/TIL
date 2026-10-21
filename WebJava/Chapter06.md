# 웹자바프로그래밍입문 6장 내용 정리

## 1. JSP와 데이터베이스 연동
- **JDBC(Java Database Connectivity)** : Java 프로그램에서 DB에 접근하기 위한 표준 API
- MariaDB용 **JDBC 드라이버**를 웹 프로젝트에 포함
- 연결 문자열과 계정 정보를 사용하여 DB에 접속

## 2. Connection과 DriverManager
- **DriverManager.getConnection()** : 데이터베이스 접속을 요청
- **Connection** : 연결된 데이터베이스를 다루는 객체
- 필요한 작업 후 연결을 종료해야 함
- **try-with-resources** : 자원을 블록 종료 시 자동으로 정리하는 방식

## 3. Statement와 SQL 실행
- **Statement** : SQL 문장을 실행하는 객체
- **executeUpdate()** : 테이블 생성, 삽입, 수정, 삭제처럼 결과 행 집합을 돌려주지 않는 명령에 사용
- **executeQuery()** : SELECT 쿼리로 조회 결과를 받을 때 사용

## 4. ResultSet
- **ResultSet** : SELECT 결과를 행 단위로 처리하는 객체
- `rs.next()` : 다음 행으로 이동하고 읽을 행이 있으면 true 반환
- `rs.getString()`, `rs.getInt()` 등으로 현재 행의 필드 값 읽기

## 5. 데이터베이스 작업 순서
1. JDBC 드라이버 준비 및 접속
2. Statement 생성
3. SQL 문장 실행
4. 필요하면 ResultSet에서 결과 읽기
5. 연결과 관련 자원 정리

## 6. 핵심 정리
- `executeUpdate()`와 `executeQuery()`의 차이
- `Connection`, `Statement`, `ResultSet`의 역할
- 접속·쿼리·조회·종료까지의 전체 흐름 이해
