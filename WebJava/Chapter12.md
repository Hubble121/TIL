# 웹자바프로그래밍입문 12장 내용 정리

## 1. 서블릿(Servlet)
- **서블릿** : Tomcat 같은 서블릿 컨테이너에서 실행되는 Java 프로그램
- JSP와 달리 `.java` 클래스 형태로 작성하며 보통 `HttpServlet`을 상속
- **서블릿 컨테이너** : 요청에 맞는 서블릿 실행, 요청·응답 객체 전달 등의 역할
- 서블릿의 경로와 HTTP 요청 방식에 따라 실행할 메서드 결정

## 2. 서블릿의 기본 구성
- **HttpServlet** : HTTP 요청을 처리하기 위한 서블릿 부모 클래스
- **@WebServlet("/HelloWorld")** : 해당 서블릿을 실행할 URL 경로 지정
- **doGet()** : GET 요청을 처리
- **doPost()** : POST 요청을 처리
- **HttpServletRequest** : 요청 경로와 입력값 등 클라이언트가 보낸 정보
- **HttpServletResponse** : 응답 내용과 이동 방식 등을 설정할 때 사용
- 강의 예시에서는 `doPost()`에서 `doGet()`을 호출하여 두 요청을 같은 코드로 처리

## 3. 서블릿에서 HTML 출력
- `response.setContentType("text/html; charset=utf-8")` : 응답 종류와 문자 인코딩 지정
- `PrintWriter out = response.getWriter()` : 응답을 출력할 객체 가져오기
- `out.println()` : 응답으로 보낼 HTML 문자열 출력
- **request.getParameter("name")** : URL이나 폼에 전달된 값 읽기

```java
response.setContentType("text/html; charset=utf-8");
PrintWriter out = response.getWriter();
out.println("<html>");
out.println("<body>서블릿에서 출력한 내용</body>");
out.println("</html>");
```

## 4. MVC 패턴
- **MVC(Model–View–Controller)** : 처리 책임을 세 부분으로 나누는 구조
- **Model** : 데이터와 이를 처리하는 프로그램 로직
- **View** : 사용자에게 보여 줄 화면
- **Controller** : 사용자 요청을 받고 Model 호출과 View 선택을 관리
- 화면 처리와 데이터 처리 코드를 구분하여 유지보수를 편리하게 함

## 5. 모델 1과 모델 2
- **모델 1** : JSP 안에 화면 출력, 요청 처리, 일부 데이터 처리 코드가 섞인 구조
- **모델 2** : MVC를 적용해 서블릿 Controller와 JSP View를 분리한 구조
- 게시판의 데이터 처리에는 **DAO**, 애플리케이션 로직에는 **Service**, 값 전달에는 **DTO**를 활용할 수 있음
- JSP에는 가급적 화면을 표시하는 코드만 남김

## 6. 컨트롤러의 주요 역할
1. 요청된 URL에서 어떤 기능을 실행할지 판단
2. 필요한 데이터 조회나 처리 기능 호출
3. 결과를 request 속성 등에 저장
4. 보여줄 JSP를 선택하여 Forward하거나 다른 경로로 Redirect
- 강의 예제에서는 하나의 컨트롤러가 `/list`, `/view`, `/insert`와 같은 경로를 구분

## 7. 요청 URL에서 명령 부분 구하기
```java
String uri = request.getRequestURI();
String conPath = request.getContextPath();
String com = uri.substring(conPath.length());
```

- **getRequestURI()** : 서버에 요청한 URI의 경로
- **getContextPath()** : 웹 애플리케이션의 컨텍스트 경로
- **substring()** : 컨텍스트 경로 뒤에 남은 `/a`, `/list` 같은 요청 경로 구하기
- `@WebServlet("/")`을 사용한 컨트롤러에서 요청 종류를 구분하는 기본 방식

## 8. Forward와 Redirect
- **Forward** : 현재 요청을 서버 내부의 JSP로 전달. request 속성을 그대로 이용할 수 있음
  - `request.getRequestDispatcher("a.jsp").forward(request, response)`
- **Redirect** : 브라우저가 새 URL로 다시 요청하게 함
  - `response.sendRedirect("b.jsp")`
- 데이터를 조회한 뒤 화면을 보여줄 때 Forward, 등록·수정 뒤 목록 등으로 이동할 때 Redirect를 사용할 수 있음

## 9. 컨트롤러의 화면 이동 처리
```java
String view = null;
if (com.equals("/a") || com.equals("/")) {
    view = "a.jsp";
} else if (com.equals("/b")) {
    view = "redirect:b.jsp";
}

if (view.startsWith("redirect:")) {
    response.sendRedirect(view.substring(9));
} else {
    request.getRequestDispatcher(view).forward(request, response);
}
```
- `view` 변수에 이동할 JSP 경로 또는 Redirect 지시문 저장
- `redirect:` 접두어에 따라 이동 방법을 구분
- 요청별로 반복되는 페이지 이동 코드를 한곳에 모을 수 있음
- 실제 프로젝트에서는 처리하지 않는 URL의 오류 처리도 고려

## 10. 개발환경과 주의사항
- 수업 PPT의 일부 예제는 `javax.servlet`을 사용하지만, **Tomcat 10 이상 환경**에서는 `jakarta.servlet` 계열을 사용
- Eclipse에서 서블릿 관련 import 오류가 나면 프로젝트의 Tomcat 런타임 설정을 확인
- `@WebServlet`의 대소문자를 포함한 경로와 브라우저에서 호출하는 URL을 일치시켜야 함

## 11. 핵심 정리
- **서블릿**은 HTTP 요청을 처리하는 Java 클래스이며 **JSP**는 화면 출력에 사용
- **Model / View / Controller**의 역할을 구분하는 것이 MVC의 핵심
- 요청 URI에서 필요한 명령 경로를 추출해 기능별로 처리
- **Forward는 같은 요청 유지**, **Redirect는 브라우저가 새 요청**
- 다음 장에서 게시판의 JSP 처리 코드를 Controller와 Service로 분리
