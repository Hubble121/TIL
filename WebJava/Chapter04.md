# 웹자바프로그래밍입문 4장 내용 정리

## 1. HTML Form
- **form 태그** : 사용자가 입력한 값을 서버로 전달하는 입력 양식
- **action** : 입력값을 전송할 주소
- **method** : GET 또는 POST와 같은 전송 방식
- **input / select / textarea** : 다양한 종류의 입력 태그
- **name** : 서버가 입력값을 구분할 때 사용하는 이름

## 2. GET과 POST
- **GET** : 요청 데이터가 URL 쿼리 문자열에 포함되어 전달됨
- **POST** : 입력 데이터를 요청 본문에 담아 전송
- GET은 URL에 매개변수가 나타날 수 있으므로 전달 방식에 따라 사용 목적을 구분

## 3. JSP에서 값 읽기
- `request.getParameter("name")` : 한 입력값을 문자열로 가져옴
- 입력된 숫자는 필요에 따라 `Integer.parseInt()` 등으로 자료형 변환
- 전달되지 않은 값은 `null`일 수 있으므로 확인이 필요

## 4. 단일 선택과 다중 선택
- **radio** : 선택지 가운데 하나 선택
- **select** : 목록에서 값 선택
- **checkbox** : 여러 항목 선택 가능
- `request.getParameterValues("name")` : 동일한 이름으로 전달된 여러 값을 배열로 가져옴

## 5. 입력 처리 흐름
1. HTML 폼에서 값 입력
2. GET/POST로 서버에 요청
3. JSP에서 `request` 객체를 통해 입력값 읽기
4. 가공한 결과를 화면에 출력

## 6. 핵심 정리
- `action`, `method`, `name`의 역할 이해
- 단일 값과 다중 값에 사용하는 요청 메서드 차이 구분
