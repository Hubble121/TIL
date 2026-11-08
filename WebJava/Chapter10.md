# 웹자바프로그래밍입문 10장 내용 정리

## 1. DAO와 DTO를 사용하는 이유
- 기존 게시판은 JSP마다 **DB 연결·데이터 처리·화면 출력** 코드가 함께 존재
- 이를 분리하여 코드 중복을 줄이고 수정·유지보수를 편리하게 만드는 것이 목적
- **DAO(Data Access Object)** : DB 접근 및 SQL 실행을 담당하는 객체
- **DTO(Data Transfer Object)** : 모듈 사이에 게시글 데이터를 전달하는 객체

## 2. DAO의 역할
- **BoardDao.java**를 JSP와 분리된 Java 클래스로 작성
- 수업에서는 `com.board.dao` 패키지에 `BoardDao` 저장
- DB 연결 코드 `getConnection()`을 공통 메서드로 분리
- JSP에서 직접 SQL을 작성하지 않고 DAO의 공개 메서드를 호출

## 3. DTO의 역할
- 수업에서는 `com.board.dto` 패키지에 `BoardDto` 작성
- 게시글 한 건의 필드들을 객체 하나에 담아 전달
- 필드 : `num`, `writer`, `title`, `content`, `regtime`, `hits`
- **Getter** : 값 조회 / **Setter** : 값 설정
- Eclipse의 getter·setter 자동 생성 기능을 활용

## 4. BoardDao의 핵심 메서드
| 메서드 | 역할 |
|---|---|
| `getConnection()` | MariaDB 연결 생성 |
| `getCurrentTime()` | 게시글 작성·수정에 사용할 시각 생성 |
| `selectOne(int num, boolean incHits)` | 글 한 건 조회 후 DTO 반환 |
| `selectList()` | 게시글 전체를 `ArrayList<BoardDto>`로 반환 |
| `insertOne(BoardDto dto)` | 게시글 추가 |
| `updateOne(BoardDto dto)` | 게시글 수정 |
| `deleteOne(int num)` | 게시글 삭제 |

## 5. 조회수 증가 여부
- `selectOne(num, true)` : 상세 글 조회 시 **조회수 증가**
- `selectOne(num, false)` : 수정 화면에서 값만 읽을 때 **조회수 유지**
- 하나의 조회 메서드라도 사용 목적에 따라 동작을 구분할 수 있음

## 6. 삭제·조회 페이지에 DAO 적용
- **delete.jsp** : `new BoardDao().deleteOne(num)` 호출
- **view.jsp** : DAO가 돌려준 `BoardDto` 객체의 getter로 화면 출력
- JSP에서 DB 연결 코드와 `ResultSet` 처리를 직접 반복하지 않음

## 7. 등록·수정 페이지에 DAO 적용
- **insert.jsp** : 입력값으로 `BoardDto` 작성 후 `insertOne(dto)` 호출
- **update.jsp** : 글 번호·입력값을 DTO에 넣고 `updateOne(dto)` 호출
- 입력값 검사와 화면 이동은 JSP에서, DB 처리는 DAO에서 담당
- 날짜·시간 생성은 DAO의 공통 메서드로 관리

## 8. write.jsp에 DAO 적용
- 새 글쓰기 모드에서는 비어 있는 DTO로 입력 폼 구성
- `num > 0`이면 수정 모드로 처리
- 수정 모드에서 `selectOne(num, false)`로 기존 글을 조회
- 폼의 `action`을 등록·수정 목적에 따라 변경

## 9. 게시글 목록에 DAO·DTO 적용
- 목록은 게시글이 여러 건이므로 DTO도 여러 개 필요
- `selectList()`의 반환 타입은 **`ArrayList<BoardDto>`**
- DAO에서 결과 행마다 DTO를 만들고 리스트에 추가
- `list.jsp`에서는 전달받은 DTO 목록을 반복 출력

## 10. 핵심 정리
- **JSP = 화면과 요청 처리 / DAO = DB 접근 / DTO = 데이터 전달**
- DAO 메서드 호출만으로 게시판의 등록·조회·수정·삭제 처리
- 한 건 조회는 `BoardDto`, 전체 목록 조회는 `ArrayList<BoardDto>` 이용
- 조회수 증가를 제어하는 `selectOne(num, incHits)`의 두 번째 인수 이해
