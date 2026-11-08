# JAVA프로그래밍응용 9주차 배운 내용 정리

## 1. 이벤트 기반 프로그래밍
- **이벤트(Event)** : 프로그램 실행 중 발생하는 사용자 입력이나 외부 동작
- **이벤트 기반 프로그래밍** : 이벤트가 발생하면 정해진 처리 코드가 실행되는 방식
- 마우스 클릭·드래그, 키보드 입력, 네트워크 데이터 수신 등이 이벤트의 예
- Java Swing GUI에서는 이벤트에 따른 동작을 **리스너(Listener)**로 구현

## 2. 이벤트 관련 용어
- **이벤트 소스(Event Source)** : 이벤트를 발생시킨 GUI 컴포넌트
- **이벤트 객체(Event Object)** : 종류·발생 위치·소스 등의 정보를 담은 객체
- **이벤트 리스너(Event Listener)** : 이벤트를 받아 처리하는 코드
- **이벤트 분배 스레드(Event Dispatch Thread)** : 이벤트가 발생하면 등록된 리스너의 메서드를 실행하는 스레드

## 3. 리스너 인터페이스
- **ActionListener** : 버튼 클릭 등 액션 이벤트 처리
  - `actionPerformed(ActionEvent e)` 메서드 구현
- **MouseListener** : 마우스 입력 이벤트 처리
  - `mousePressed()`, `mouseReleased()`, `mouseClicked()`, `mouseEntered()`, `mouseExited()`
- 인터페이스를 직접 구현하면 필요한 추상 메서드를 모두 작성해야 함

## 4. 이벤트 리스너 등록 과정
1. `implements ActionListener` 등으로 이벤트를 처리할 클래스 정의
2. 이벤트 종류에 맞는 콜백 메서드 구현
3. 리스너 객체 생성 후 GUI 컴포넌트에 등록
4. 이벤트가 발생하면 등록한 메서드 실행

```java
class MyActionListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        // 버튼을 누르면 실행할 코드
    }
}

// GUI 컴포넌트에서 리스너 등록
btn.addActionListener(new MyActionListener());
```

## 5. 리스너 작성 방법
- **독립 클래스** : 별도 클래스로 작성하며 여러 곳에서 재사용할 때 적합
- **내부 클래스(Inner Class)** : 특정 GUI 클래스의 멤버에 접근하기 편리
- **익명 클래스(Anonymous Class)** : 리스너 코드가 짧고 특정 위치에서만 필요할 때 사용
- `e.getSource()` : 이벤트를 발생시킨 컴포넌트를 가져옴. 반환 타입은 `Object`

## 6. 마우스 이벤트와 어댑터 클래스
- `MouseListener`를 직접 구현하면 다섯 메서드를 모두 구현해야 함
- **Adapter Class** : 이벤트 메서드를 기본 구현한 클래스
- **MouseAdapter** : 필요한 마우스 메서드만 재정의하여 사용할 수 있음
- 예 : 마우스를 눌렀을 때만 처리하려면 `mousePressed(MouseEvent e)` 구현
- 추상 메서드가 하나인 리스너는 별도 어댑터 클래스가 필요하지 않음

## 7. 실습 개념
- **JFrame** 안에 버튼과 JLabel 등 컴포넌트 배치
- 버튼 클릭 후 라벨 내용이나 창 제목을 바꾸는 이벤트 처리
- 버튼의 이벤트 소스를 구분하여 서로 다른 동작 실행
- 마우스 위치와 클릭 이벤트에 따라 화면에 내용 표시
- 9주차 미션 : 간단한 자기소개 프로그램을 만들고 글꼴·색상 등 출력 형태 변경

## 8. 핵심 정리
- **이벤트 발생 → 리스너 실행 → GUI 상태 변경** 흐름 이해
- ActionListener와 MouseListener가 처리하는 이벤트와 메서드 구분
- 독립·내부·익명 클래스의 차이, Adapter를 사용하는 목적 기억
