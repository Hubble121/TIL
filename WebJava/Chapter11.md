# 웹자바프로그래밍입문 11장(11주차) 배운 내용 정리

## 1. EL과 JSTL의 목적
- **EL(Expression Language)** : JSP 표현식 대신 `${...}` 형태로 저장된 값이나 계산 결과를 출력하는 기술
- **JSTL(JSP Standard Tag Library)** : 조건문·반복문 등 자바 제어 코드를 JSP 태그로 작성할 수 있도록 하는 표준 태그 라이브러리
- **학습 목표** : HTML 영역에 섞여 있는 Java 코드를 줄여 화면 코드의 가독성을 높이는 것

## 2. EL의 기본 사용법
- **EL 기본 형식** : `${속성명}`
- JSP 내장 객체에 저장된 속성을 읽거나 수식·비교 연산에 활용 가능
- 예 : `session.setAttribute("age", 20)`으로 저장했다면 `${age}` → `20`
- **연산 예시** : `${age + 5}` → `25`, `${age == 20}` → `true`
- 일반적인 Java 지역변수가 아닌 **JSP 속성으로 저장된 값**을 참조한다는 점에 주의

## 3. EL에서 사용하는 4가지 저장 범위(Scope)
| 범위 | 특징 |
|---|---|
| **pageContext** | 현재 JSP 페이지에서 사용 |
| **request** | 하나의 요청을 처리하는 동안 사용. 포워드 시 공유 가능 |
| **session** | 사용자별 세션이 유지되는 동안 사용 |
| **application** | 웹 애플리케이션이 운영되는 동안 공유 |

- 속성명만 적으면 **page → request → session → application** 순서로 검색
- 범위를 명시하려면 `${requestScope.name}`, `${sessionScope.age}` 등 사용
- 내장 객체는 `setAttribute()`와 `getAttribute()`로 속성을 저장·조회

## 4. 포워드(Forward)와 리다이렉트(Redirect)
- **리다이렉트** : 서버가 다른 URL로 요청하도록 브라우저에 응답 → **새 요청**이 발생하고 주소창 URL이 바뀜
  - `response.sendRedirect("b.jsp");`
- **포워드** : 서버 내부에서 다른 페이지로 요청 처리 전달 → **같은 request**를 사용하고 주소창 URL은 유지
  - JSP 액션 태그 : `<jsp:forward page="b.jsp"/>`
  - Java 코드 : `request.getRequestDispatcher("b.jsp").forward(request, response);`
- 포워드와 리다이렉트는 **요청 객체를 유지하는지**를 중심으로 구분

## 5. 자바 빈(Java Bean)과 EL
- **Java Bean** : 데이터를 보관하고 getter·setter 등으로 속성에 접근하는 Java 클래스
- **DTO** : 프로그램 계층 간 데이터를 전달하는 역할에 초점을 맞춘 명칭
- 자바 빈의 EL 속성 이름은 보통 **getter·setter 메서드 이름을 기준**으로 결정
- `getName()`이 있으면 `name` 속성에 접근 가능
- `request.setAttribute("score1", score)`로 등록한 뒤 `${score1.name}`, `${score1.kor}`처럼 출력

## 6. 게시판에 EL 적용
- 화면 출력 담당 파일인 **view.jsp / write.jsp / list.jsp**의 표현식 부분을 EL로 변경
- `BoardDao`에서 `BoardDto`를 조회한 뒤 `request.setAttribute("msg", dto)`로 등록
- **글 상세** : `${msg.title}`, `${msg.writer}`, `${msg.content}`, `${msg.hits}`
- **요청 매개변수** : `${param.num}`으로 전달된 글 번호에 접근
- **글 작성·수정** : `request.setAttribute("action", action)`으로 저장한 전송 경로를 `action="${action}"`으로 지정
- EL만 적용한 목록 화면에는 Java 반복문이 남을 수 있음 → JSTL로 개선

## 7. JSTL 설치와 사용 준비
- **JSTL**은 별도 라이브러리가 필요하며, 강의자료의 Tomcat 10 이상 실습에서는 Jakarta JSTL API·구현 JAR을 `WEB-INF/lib`에 추가
- JSP 파일 상단에 **taglib 지시자**를 작성해 코어 태그 사용
- 강의자료의 코어 라이브러리 선언 예 :
```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```
- **참고** : 사용하는 Tomcat·Jakarta JSTL 버전에 따라 태그 URI 등 설정이 달라질 수 있으므로 설치한 라이브러리 기준으로 확인

## 8. JSTL 주요 태그
- **`<c:if>`** : 조건이 참일 때만 내부 내용을 실행
  - `<c:if test="${age >= 20}">성인입니다.</c:if>`
- **`<c:choose>` / `<c:when>` / `<c:otherwise>`** : if–else if–else와 유사한 조건 분기
- **`<c:forEach>`** : 숫자 범위 또는 배열·리스트 반복
  - `<c:forEach var="count" begin="0" end="30" step="3">`
  - `<c:forEach var="data" items="${dataList}">`
- HTML 영역에 Java의 `if`, `for`를 직접 작성하는 대신 태그로 표현

## 9. 게시판 목록에 JSTL 적용
- DAO에서 게시글 목록을 가져와 `request.setAttribute("msgList", new BoardDao().selectList())`로 등록
- `<c:forEach>`를 사용해 `BoardDto` 목록을 순회하고 EL로 각각의 필드 출력

```jsp
<c:forEach var="msg" items="${msgList}">
    <tr>
        <td>${msg.num}</td>
        <td>${msg.title}</td>
        <td>${msg.writer}</td>
        <td>${msg.hits}</td>
    </tr>
</c:forEach>
```

- 기존 JSP의 Java `for` 반복문을 **JSTL 반복 태그**로 바꾸는 과정이 핵심

## 10. 핵심 정리
- **EL은 값 출력**, **JSTL은 조건·반복 제어**에 주로 사용
- JSP 속성의 생명주기는 **page / request / session / application**에 따라 다름
- **Forward는 같은 요청 유지**, **Redirect는 브라우저의 새 요청 발생**
- **Java Bean의 속성명은 getter·setter 기준**으로 접근
- **DAO·DTO → request 속성 → EL·JSTL로 화면 표시** 흐름으로 게시판 구조 이해
