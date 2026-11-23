# 웹자바프로그래밍입문 13장 내용 정리

## 1. MVC 게시판의 목표
- 기존 JSP 게시판의 **화면 출력·요청 처리·DB 로직**을 분리
- **Controller** : 요청에 따라 어떤 기능을 실행하고 어느 화면을 보여줄지 결정
- **View(JSP)** : 전달받은 데이터를 EL·JSTL로 화면에 표시
- **Model** : 서비스(Service), DAO, DTO로 구성하여 애플리케이션 로직과 데이터를 관리

## 2. BoardController 준비
- 수업에서는 `com.board.controller.BoardController` 클래스를 서블릿으로 생성
- `@WebServlet("/")`으로 여러 요청을 하나의 컨트롤러에서 처리
- `getRequestURI()`, `getContextPath()`, `substring()`을 사용해 `/list`, `/view` 등 명령 구분
- 각 기능의 처리 결과에 따라 Forward 또는 Redirect 선택

## 3. 게시글 목록(list) 처리
- `/list` 또는 `/` 요청을 받으면 DAO에서 게시글 목록 조회
- `request.setAttribute("msgList", new BoardDao().selectList())`로 결과 전달
- `list.jsp`는 `<c:forEach>`로 목록을 순회하고 `${msg.num}`, `${msg.title}` 등의 EL로 출력
- 데이터 조회 기능이 컨트롤러로 이동했으므로 **list.jsp를 직접 요청하지 않고 /list로 접근**해야 함

## 4. 게시글 상세(view) 처리
- 요청 경로 `/view`에서 `request.getParameter("num")`으로 글 번호 받기
- `selectOne(num, true)`로 게시글 조회 및 조회수 처리
- `request.setAttribute("msg", dto)`로 DTO 전달
- `view.jsp`에서는 EL로 제목·작성자·작성일·조회수·내용 표시
- 글의 공백과 줄바꿈을 HTML 출력 형태에 맞게 처리하는 예제 포함

## 5. 글쓰기와 수정(write) 처리
- **새 글쓰기** : 기본 DTO를 준비해 입력 폼으로 이동
- **글 수정** : 글 번호가 있으면 `selectOne(num, false)`로 기존 글을 조회하고 폼에 표시
- `request.setAttribute("msg", dto)` : 화면에 채울 데이터 전달
- `request.setAttribute("action", action)` : 입력 폼의 저장 대상 경로 전달
- `write.jsp`는 `${action}`, `${msg.title}`, `${msg.writer}`, `${msg.content}` 등으로 구성

## 6. INSERT / UPDATE / DELETE의 컨트롤러 이동
- 기존 `insert.jsp`, `update.jsp`, `delete.jsp`의 DB 처리 코드를 Controller 쪽으로 이동
- 각각 독자적인 출력 화면이 없으므로 기능 이전 후 처리용 JSP 파일 삭제
- `/insert`, `/update`, `/delete` 요청에 따라 DAO를 이용해 게시글 처리
- 완료 후 `view = "redirect:list"`처럼 목록으로 이동
- 기존 JSP 파일명으로 이동하던 버튼과 링크는 **/list, /view, /write 등의 명령 경로**로 변경

## 7. 오류 처리 페이지(errorBack.jsp)
- 입력값이 비었을 때 Controller에서 직접 JavaScript를 출력하지 않도록 구성
- `request.setAttribute("errorMessage", "모든 항목이 빈칸 없이 입력되어야 합니다.")`로 메시지 저장
- `errorBack.jsp`는 EL로 오류 메시지를 표시하고 이전 페이지로 돌아가는 역할
- **요청 처리·입력 검사**와 **화면 출력**의 책임을 분리

## 8. Service 계층의 필요성
- Controller에 데이터 검사나 상세 비즈니스 로직까지 있으면 코드가 복잡해짐
- **Service** : Controller와 DAO 사이에서 애플리케이션 로직 담당
- **DAO** : 직접적인 데이터베이스 접근·SQL 처리
- **DTO** : 게시글 데이터 값을 계층 사이에 전달
- 구조 : **Controller → Service → DAO → DB**, 처리 결과는 DTO 등으로 View에 전달
- 웹 화면이 바뀌더라도 Model의 로직을 재사용하기 쉽게 구성

## 9. BoardService의 주요 메서드
| 메서드 | 역할 |
|---|---|
| `getMsgList()` | 게시글 목록 조회 |
| `getMsg(int num)` | 게시글 하나 조회 및 출력용 데이터 가공 |
| `getMsgForWrite(int num)` | 수정 화면을 위한 글 조회(조회수 미증가) |
| `writeMsg(writer, title, content)` | 입력 검사 후 글 등록 |
| `updateMsg(writer, title, content, num)` | 입력 검사 후 글 수정 |
| `deleteMsg(int num)` | 글 삭제 |

- 수업 예제에서 `BoardService`가 `BoardDao`를 호출
- 입력이 누락되면 Service에서 `Exception`을 던지고 Controller에서 오류 처리

## 10. Service 적용 후 Controller
- Controller는 URL 판단, 파라미터 수신, Service 호출, View 선택에 집중
- `new BoardService().getMsgList()` 등으로 조회 기능 호출
- 게시글 등록·수정 시 `try-catch`로 Service의 입력 검사 예외 처리
- `errorMessage`를 request에 넣고 `errorBack.jsp`로 Forward 가능
- 기능이 정상 완료되면 목록 등으로 Redirect

## 11. 핵심 정리
- **JSP(View)**에서는 데이터를 출력하고 **Controller**는 요청 흐름만 관리
- **Service**는 입력 검사 등 비즈니스 로직, **DAO**는 DB 접근, **DTO**는 데이터 전달
- `list.jsp`를 직접 실행하는 대신 **/list 요청 → Controller → JSP Forward**로 접근
- 글쓰기·수정·삭제는 Controller에서 기능을 수행하고 완료 후 Redirect
- MVC를 적용하면 역할이 분리되어 수정과 유지보수가 쉬워짐
