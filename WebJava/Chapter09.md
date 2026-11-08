# 웹자바프로그래밍입문 9장 내용 정리

## 1. 게시판 프로그램의 구성
- 게시글 **목록·등록·상세 조회·수정·삭제(CRUD)**를 JSP와 DB로 구현
- 화면에 내용을 보여주는 파일과 데이터베이스 처리용 파일의 역할 구분

| JSP 파일 | 주요 역할 |
|---|---|
| `list.jsp` | 등록된 게시글 목록 출력 |
| `view.jsp` | 글 번호를 받아 게시글 상세 조회 |
| `write.jsp` | 새 글쓰기와 수정에 공통으로 사용하는 입력 폼 |
| `insert.jsp` | 새로운 글 INSERT |
| `update.jsp` | 기존 글 UPDATE |
| `delete.jsp` | 특정 글 DELETE |

## 2. 게시판 테이블 구조
- **board 테이블** : `num`, `writer`, `title`, `content`, `regtime`, `hits`
- `num` : `auto_increment primary key`를 이용하는 글 번호
- `writer` : 작성자 / `title` : 제목 / `content` : 내용
- `regtime` : 작성 시각 / `hits` : 조회수
- 게시글을 구분하는 기준은 **글 번호(`num`)**

## 3. 게시글 목록(list.jsp)
- JDBC로 DB 접속 후 게시글을 가져오는 SELECT 실행
- `select * from board order by num desc` : 글 번호가 큰 순서로 정렬
- `ResultSet`을 반복하며 제목·작성자·작성일·조회수 출력
- 제목 링크는 `view.jsp?num=글번호` 형식으로 상세 화면 연결

## 4. 새 글 작성(write.jsp / insert.jsp)
- `write.jsp`에서 작성자·제목·내용을 입력받고 POST 요청 전송
- `request.getParameter()`로 입력값 읽기
- 필수 항목이 비어 있는지 검사하고 오류 안내
- `insert.jsp`에서 글 데이터를 INSERT하고 작성 시각과 초기 조회수 설정
- 저장 완료 후 목록으로 이동

## 5. 글 상세 조회(view.jsp)
- URL의 `num` 값을 받아 특정 게시글 조회
- 조회수(`hits`)를 증가시키고 제목·내용·작성자·작성 시각 등을 출력
- HTML 화면에서는 여러 공백·줄바꿈이 그대로 표시되지 않을 수 있으므로 표시 방식 고려
- 상세 화면에서 수정·삭제 버튼을 통해 다음 기능으로 이동

## 6. 글 수정(write.jsp / update.jsp)
- `write.jsp`를 **신규 등록과 수정에 공통 사용**
- 글 번호가 있으면 해당 글을 읽어 폼의 초기값을 설정
- 신규 작성은 `insert.jsp`, 수정은 `update.jsp?num=...`로 전송
- `update.jsp`에서 전달받은 내용을 UPDATE 처리

## 7. 글 삭제(delete.jsp)
- 요청받은 `num`에 해당하는 게시글을 DB에서 DELETE
- 삭제 처리 후 목록 화면으로 이동
- 실습의 처리 파일은 별도 화면 출력을 최소화하고 DB 작업에 집중

## 8. 주요 화면 이동 흐름
1. **목록 → 상세 보기** : `list.jsp → view.jsp?num=...`
2. **목록 → 새 글 저장** : `list.jsp → write.jsp → insert.jsp`
3. **상세 → 수정** : `view.jsp → write.jsp?num=... → update.jsp`
4. **상세 → 삭제** : `view.jsp → delete.jsp → list.jsp`

## 9. 핵심 정리
- 각 JSP의 화면 출력과 DB 처리 역할 구분
- `num` 전달과 `SELECT / INSERT / UPDATE / DELETE`의 관계 이해
- 목록 반복 출력, 새 글·수정 폼 재사용, 조회수 갱신 흐름 복습
