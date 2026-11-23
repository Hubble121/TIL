# JAVA프로그래밍응용 10주차 배운 내용 정리

## 1. 키보드 이벤트 처리
- **키 이벤트(Key Event)** : 사용자가 키보드를 누르거나 떼는 동작에서 발생하는 이벤트
- **KeyListener** : 키 입력에 따라 실행할 메서드를 정의한 이벤트 리스너 인터페이스
- 이벤트를 처리할 컴포넌트에 `addKeyListener()`로 리스너 등록
- 9주차의 마우스·버튼 이벤트 처리와 같은 **이벤트 발생 → 리스너 실행 → 화면 변경** 흐름을 사용

## 2. 키보드 이벤트의 세 가지 메서드
- **keyPressed(KeyEvent e)** : 키를 누를 때 실행
- **keyTyped(KeyEvent e)** : 문자 입력이 발생할 때 실행
- **keyReleased(KeyEvent e)** : 눌렀던 키를 뗄 때 실행
- 문자 입력에서는 일반적으로 `keyPressed() → keyTyped() → keyReleased()` 순서로 호출
- 방향키·기능키 등은 문자 입력이 아니므로 `keyTyped()`가 발생하지 않을 수 있음

## 3. 포커스(Focus)
- **포커스** : 키보드 이벤트를 받을 대상으로 선택된 컴포넌트의 상태
- 키 리스너를 등록했더라도 해당 컴포넌트가 포커스를 받지 않으면 키 이벤트를 처리하지 못할 수 있음
- **setFocusable(true)** : 컴포넌트가 포커스를 받을 수 있도록 설정
- **requestFocus()** : 해당 컴포넌트에 포커스를 요청
- 프레임의 컨텐트팬에 키 리스너를 등록하고 포커스를 주는 실습에 활용

## 4. KeyEvent로 입력한 키 구분
- **KeyEvent** : 발생한 키 이벤트의 정보를 담은 객체
- **getKeyChar()** : 입력된 문자를 가져올 때 사용
- **getKeyCode()** : 키 코드를 가져와 기능키나 방향키 등을 구분
- **가상 키(Virtual Key)** : `KeyEvent.VK_ENTER`, `VK_F5`, `VK_LEFT`, `VK_RIGHT`처럼 선언된 키 상수
- 예 : `e.getKeyCode() == KeyEvent.VK_ENTER`이면 Enter 키 입력으로 판단

## 5. KeyListener와 KeyAdapter
- **KeyListener**를 직접 구현하면 키 이벤트 메서드 3개를 구현해야 함
- **KeyAdapter**는 필요한 메서드만 재정의할 수 있도록 만든 어댑터 클래스
- 특정 키 입력만 처리하려면 `keyPressed()` 하나를 재정의하여 사용 가능

```java
addKeyListener(new KeyAdapter() {
    @Override
    public void keyPressed(KeyEvent e) {
        if (e.getKeyCode() == KeyEvent.VK_ENTER) {
            System.out.println("Enter 입력");
        }
    }
});
```

## 6. 키보드 이벤트 활용 실습
- **Enter 키 입력 시 컨텐트팬의 배경색을 무작위로 변경**하는 프로그램
- `getContentPane()` : JFrame의 컨텐트팬을 가져오는 메서드
- `setBackground(new Color(r, g, b))` : 배경색 변경
- `Math.random()`을 이용하여 RGB 각 값(0~255)을 생성
- 리스너 등록과 포커스 설정이 실제 키 입력 처리에 영향을 미침

## 7. Swing 컴포넌트 공통 메서드
- **setForeground(Color)** : 전경색 설정
- **setBackground(Color)** : 배경색 설정
- **setFont(Font)** : 글꼴 설정
- **setEnabled(boolean)** : 컴포넌트 활성화·비활성화
- **setVisible(boolean)** : 화면 표시·숨김
- **getWidth() / getHeight()** : 컴포넌트의 크기 확인
- **setLocation(x, y) / setSize(w, h)** : 위치·크기 지정
- **add() / remove() / removeAll()** : 컨테이너의 자식 컴포넌트 관리

## 8. JLabel과 ImageIcon
- **JLabel** : 문자열 또는 이미지를 화면에 표시하는 컴포넌트
- **ImageIcon** : 파일에서 이미지를 읽어 아이콘 형태로 사용
- 강의자료의 이미지 예시 형식 : `new ImageIcon("p1.png")`
- **setIcon()** : JLabel 등 컴포넌트에 표시된 이미지 변경
- **png, gif, jpg** 등의 이미지 파일을 사용할 수 있음
- 이미지 파일의 경로가 실제 프로젝트 실행 위치와 일치하는지 확인

## 9. JButton과 JPanel 활용
- **JButton** : 클릭할 수 있는 버튼이며 클릭 시 Action 이벤트 발생
- **JPanel** : 여러 컴포넌트를 모아 배치하는 컨테이너
- `addActionListener()`로 버튼 동작 등록
- 강의 예제 : 버튼을 누르면 `JLabel.setIcon()`으로 다른 사진 표시
- 이전·다음 버튼을 통해 이미지 배열의 인덱스를 변경하는 **사진 뷰어** 실습

## 10. 핵심 정리
- 키보드 이벤트를 받으려면 **KeyListener 등록과 포커스**를 확인
- `getKeyChar()`는 문자, `getKeyCode()`는 기능키·방향키 구분에 활용
- **JLabel + ImageIcon**으로 이미지를 표시하고 **JButton** 이벤트로 변경
- **JPanel**과 여러 컴포넌트를 조합하여 화면을 구성
