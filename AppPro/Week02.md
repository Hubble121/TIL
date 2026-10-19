# 앱프로그래밍 2주차 배운 내용 정리

## 1. 프로그래밍과 엔지니어링
- **Programming** : 코드를 작성하여 기능을 구현
- **Engineering** : 설계·개발·테스트·유지보수를 포함한 제품 생애주기를 관리
- 개발환경 설정과 오류 진단도 소프트웨어 엔지니어링의 일부
- **Verification(검증)** : 정해진 요구와 규칙대로 만들고 있는지 확인
- **Validation(타당성 확인)** : 사용자가 필요로 하는 것을 만들고 있는지 확인

## 2. 선언형 UI와 Flutter Widget
- **명령형 UI** : 화면 요소를 단계별 명령으로 직접 변경하는 방식
- **선언형 UI** : 현재 상태에서 어떤 화면을 보여 줄지 기술하는 방식
- Flutter는 Widget 조합으로 UI를 선언하고 상태가 변하면 화면을 다시 구성
- Widget은 화면 구성에 사용하는 불변 설정값

## 3. Flutter의 주요 트리
- **Widget Tree** : 무엇을 만들지 나타내는 Widget 구성
- **Element Tree** : Widget 인스턴스와 화면의 관계 및 상태를 관리하는 구조
- **RenderObject Tree** : 화면 요소의 레이아웃과 그리기를 담당하는 구조
- 세 트리는 화면을 서로 다른 관점에서 나타냄

## 4. Flutter 구조와 생명주기
- **Framework(Dart)** : Widget, Material 등 앱을 구성하는 기능
- **Engine(C/C++)** : 렌더링과 런타임 관련 저수준 기능
- **Embedder(Platform)** : Android·Windows 등 운영체제와 연결
- 앱 생명주기는 `resumed`, `inactive`, `hidden`, `paused`, `detached` 등의 상태로 구분
- `State` 객체는 초기화, `build()`, `setState()`, `dispose()` 등과 관련된 생명주기를 가짐

## 5. Flutter 개발환경 구성
- **Dart** : 언어
- **Flutter SDK** : 프레임워크와 개발 도구
- **VS Code** : 코드 편집과 디버깅
- **Android SDK** : Android 빌드·실행 관련 도구
- **Git** : 변경 이력 관리
- 개발환경은 설치, 진단, 실행을 별도로 확인해야 함

## 6. 주요 Flutter 명령어
```bash
flutter --version
flutter doctor -v
flutter devices
flutter create my_app
flutter run
```

- `flutter --version` : Flutter 버전 확인
- `flutter doctor -v` : 개발환경 및 필요한 도구 점검
- `flutter devices` : 사용할 수 있는 실행 대상 확인
- `flutter create` : 프로젝트 생성
- `flutter run` : 프로젝트 실행

## 7. 오류 진단 방법
- 개발환경 문제는 **실행 대상 확인 → 첫 번째 오류 확인 → 명령어와 로그 기록** 순서로 접근
- **PATH 오류** : Flutter 명령어를 찾지 못하는 경우 환경변수 점검
- **Android SDK 오류** : SDK 설치와 경로 확인
- **라이선스 오류** : `flutter doctor --android-licenses` 실행 여부 확인
- **기기 인식 오류** : 에뮬레이터 실행 또는 USB 디버깅 연결 확인
- 경고가 나왔다고 모든 항목을 무조건 설치하기보다 실제 목표 플랫폼에 필요한지 먼저 판단

## 8. 첫 Flutter 프로젝트
- `lib/main.dart` : 앱의 진입점과 기본 화면 코드
- 프로젝트 생성 후 실행 대상을 선택하여 앱 실행
- 첫 화면이 정상 표시되는지 확인하고 코드를 일부 변경해 결과 비교

## 9. Hot Reload, Hot Restart, Restart
- **Hot Reload** : 실행 상태를 가능한 한 유지하면서 변경한 코드를 빠르게 반영
- **Hot Restart** : 앱 상태를 초기화하고 Dart 실행을 다시 시작
- **Full Restart** : 앱 실행을 완전히 다시 시작
- 수정 내용과 상태 유지 여부에 따라 적절한 방법을 선택
- `setState()`는 앱 상태를 변경할 때 화면 갱신을 알리는 메서드로 Hot Reload와 역할이 다름

## 10. 첫 Commit과 학습 기록
- 실행 결과와 수정 전후 차이를 확인한 후 Git으로 기록
- **Commit** : 변경 사항을 하나의 이력으로 저장
- 커밋에는 어떤 내용을 수정했고 무엇을 확인했는지 남기는 것이 좋음

## 11. 핵심 정리
- Flutter 개발은 설치만으로 끝나지 않고 진단과 실제 실행까지 확인해야 함
- Flutter는 현재 상태에 따른 선언형 UI를 구성
- Widget·Element·RenderObject의 역할은 서로 다름
- `flutter doctor -v`의 진단 내용과 실제 실행 결과를 구분하여 확인
- Hot Reload와 Hot Restart의 차이를 이해하고 변경 이력을 Git에 기록
