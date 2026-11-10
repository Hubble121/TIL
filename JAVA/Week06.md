# JAVA프로그래밍응용 6주차 배운 내용 정리

## 1. GUI 프로그래밍의 개념
- **GUI(Graphical User Interface)** : 버튼, 텍스트 입력창 등의 그래픽 컴포넌트로 구성하는 사용자 인터페이스
- 사용자가 화면의 컴포넌트를 통해 프로그램을 사용할 수 있도록 구성
- Java에서는 **AWT**와 **Swing**을 이용해 GUI를 작성

## 2. AWT와 Swing
- **AWT(Abstract Windowing Toolkit)** : Java 초기부터 제공된 GUI 라이브러리
  - 운영체제의 GUI 기능을 활용하는 **중량 컴포넌트(Heavyweight Component)** 사용
- **Swing** : AWT를 기반으로 작성된 Java GUI 라이브러리
  - 다양한 컴포넌트 제공, **경량 컴포넌트(Lightweight Component)** 중심
  - `JButton`, `JLabel`, `JTextField`처럼 이름이 **J**로 시작하는 컴포넌트 사용
- Swing의 대표 패키지 : `javax.swing`

## 3. 컨테이너와 컴포넌트
- **컨테이너(Container)** : 다른 컴포넌트를 포함할 수 있는 GUI 객체
  - **JFrame** : 독립적인 윈도우를 만드는 최상위 컨테이너
  - **JPanel** : 여러 컴포넌트를 하나의 영역으로 묶는 컨테이너
  - **JDialog** : 대화상자를 표현하는 컨테이너
- **컴포넌트(Component)** : 화면에 표시하는 버튼, 레이블, 텍스트 필드 등의 요소
- GUI는 **최상위 컨테이너 → 내부 컨테이너 → 컴포넌트**의 포함 관계로 구성

## 4. GUI 프로그램 작성 절차
1. `JFrame` 객체를 만들거나 `JFrame`을 상속받는 클래스 작성
2. `setTitle()`로 창 제목 지정
3. `setSize()`로 창 크기 지정
4. `JButton`, `JLabel` 등 컴포넌트를 생성하고 `add()`로 추가
5. `setVisible(true)`로 창 표시
- `main()`에서 윈도우 객체를 생성하여 실행

### JFrame 기본 형태
```java
import javax.swing.*;

public class MyFrame extends JFrame {
    MyFrame() {
        setTitle("GUI 프로그램");
        setSize(300, 150);
        add(new JButton("확인"));
        setVisible(true);
    }

    public static void main(String[] args) {
        new MyFrame();
    }
}
```

## 5. 배치관리자(Layout Manager)
- **배치관리자** : 컨테이너에 추가된 컴포넌트의 위치와 크기를 결정하는 객체
- 컨테이너 크기가 바뀌면 컴포넌트의 배치도 다시 계산될 수 있음
- `setLayout(new FlowLayout())`처럼 배치관리자 객체를 전달하여 변경
- **기본 배치관리자**
  - `JFrame`의 콘텐츠 영역 : **BorderLayout**
  - `JPanel` : **FlowLayout**

## 6. FlowLayout
- 컴포넌트를 **왼쪽에서 오른쪽으로 순서대로 배치**
- 공간이 부족하면 다음 줄로 배치
- `new FlowLayout()` : 기본 정렬과 간격 사용
- `new FlowLayout(align, hGap, vGap)`
  - **align** : LEFT, RIGHT, CENTER 정렬
  - **hGap / vGap** : 수평·수직 간격

## 7. BorderLayout
- 컨테이너를 **NORTH / SOUTH / EAST / WEST / CENTER**의 다섯 영역으로 구분
- `add(component, BorderLayout.NORTH)`처럼 영역을 지정하여 배치
- 영역을 지정하지 않고 추가하면 **CENTER**에 배치
- `JFrame`의 기본 배치관리자

## 8. GridLayout
- 컨테이너를 **행과 열로 구성된 같은 크기의 격자**로 나누어 컴포넌트 배치
- 컴포넌트는 왼쪽에서 오른쪽, 위에서 아래로 순서대로 추가
- `new GridLayout(rows, cols)` : 행과 열의 수 지정
- `new GridLayout(rows, cols, hGap, vGap)` : 칸 사이 간격도 지정
- 계산기 버튼이나 피아노 건반처럼 규칙적인 배치에 사용

## 9. JPanel과 주요 컴포넌트
- **JPanel** : 여러 컴포넌트를 묶어 다른 컨테이너에 추가할 수 있는 패널
- **JLabel** : 수정할 수 없는 텍스트 또는 이미지 표시
- **JTextField** : 한 줄의 텍스트를 입력받는 영역
- **JButton** : 사용자가 클릭할 수 있는 버튼
- 컴포넌트를 패널에 추가한 뒤 패널을 `JFrame`에 추가하는 구조도 사용 가능

## 10. 핵심 정리
- Java GUI는 **컨테이너와 컴포넌트의 조합**으로 구성
- `JFrame`으로 창을 만들고 `add()`로 컴포넌트 배치
- 배치관리자는 **FlowLayout(흐름) / BorderLayout(5개 영역) / GridLayout(격자)**로 구분
- `JFrame`과 `JPanel`의 기본 배치관리자가 서로 다름
- `JLabel`, `JTextField`, `JButton`의 용도 구분
