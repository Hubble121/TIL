# JAVA프로그래밍응용 11주차 배운 내용 정리

## 1. 프로세스와 스레드
- **프로세스(Process)** : 메모리를 할당받아 실행 중인 프로그램
- **스레드(Thread)** : 프로그램 내부에서 작업을 실행하는 흐름의 단위
- **멀티스레딩(Multi-threading)** : 하나의 프로그램에서 여러 스레드를 실행하는 기법
- 하나의 Java 프로그램은 적어도 `main()`을 실행하는 **메인 스레드**를 가지고 있음

## 2. Java 스레드와 JVM
- **JVM(Java Virtual Machine)** : Java 프로그램과 스레드 실행을 관리하는 환경
- 메인 스레드는 `main()`에서 코드를 순차적으로 실행
- 추가적인 스레드를 만들어 여러 작업의 실행을 겹치게 할 수 있음
- 스레드 이름, ID, 우선순위 등은 스레드 정보에 해당

## 3. 스레드를 만드는 두 가지 방법
- **Thread 클래스 상속** : `extends Thread`로 클래스를 만들고 `run()` 재정의
- **Runnable 인터페이스 구현** : `implements Runnable`로 작업을 정의한 후 `Thread` 객체에 전달
- 수업에서는 **Runnable 방식이 더 일반적**이라고 설명
  - Java 클래스는 다른 클래스를 하나만 상속할 수 있으므로 `Thread`를 상속하면 상속 선택이 제한됨

## 4. Thread 클래스 상속 예제
```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
    }
}

MyThread t = new MyThread();
t.start();
```

- **run()** : 스레드가 수행할 작업을 작성하는 메서드
- **start()** : 새 스레드의 실행을 시작하도록 요청
- 객체를 생성하기만 하면 스레드가 자동으로 시작되는 것은 아님
- `run()`을 직접 호출하는 것과 `start()`로 시작하는 것은 다름

## 5. Runnable 인터페이스 구현 예제
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
    }
}

Thread t = new Thread(new MyRunnable());
t.start();
```

- 작업 내용은 `Runnable.run()`에 정의
- 실행을 담당하는 `Thread` 객체를 별도로 생성하여 시작

## 6. Thread의 주요 메서드
- **start()** : 스레드 시작
- **run()** : 실행할 작업 정의
- **sleep(ms)** : 지정한 밀리초 동안 현재 스레드의 실행을 일시 정지
- **join()** : 다른 스레드가 종료될 때까지 기다림
- **interrupt()** : 스레드에 인터럽트를 요청
- **getName() / getId()** : 이름과 ID 확인
- **currentThread()** : 현재 실행 중인 스레드 확인
- **getState()** : 스레드 상태 확인

## 7. 멀티스레드 실행 특징
- 수업 예제 : `for`문으로 세 개의 스레드를 시작하여 각 스레드가 1부터 5까지 출력
- 또 다른 예제 : 대문자와 소문자를 출력하는 작업을 두 스레드로 실행
- `Thread.sleep()`으로 실행을 잠시 지연하면 출력이 서로 섞여 나타나는 모습을 확인할 수 있음
- **여러 스레드의 실행 순서는 항상 일정하지 않음**

## 8. 스레드 동기화가 필요한 이유
- **공유 데이터** : 여러 스레드가 함께 접근하는 정보나 자원
- 여러 스레드가 동시에 같은 자원에 접근하면 실행 결과가 예상과 달라질 수 있음
- **동기화(Synchronization)** : 공유 자원에 접근하는 순서와 범위를 조절하는 기법
- 강의 예시 : 두 스레드가 하나의 프린터에 동시에 문자열을 출력하면 글자가 섞일 수 있음

## 9. synchronized와 임계 영역
- **synchronized** : 한 번에 한 스레드만 특정 객체의 모니터를 획득하여 실행할 수 있도록 하는 키워드
- **임계 영역(Critical Section)** : 공유 자원에 배타적으로 접근해야 하는 코드 영역
- **동기화 메서드** 또는 **동기화 블록**으로 사용

```java
class SharedPrinter {
    synchronized void print(String text) {
        for (int i = 0; i < text.length(); i++) {
            System.out.print(text.charAt(i));
        }
        System.out.println();
    }
}
```

- 같은 `SharedPrinter` 객체를 사용하는 스레드들이 `print()`를 호출하면 한 스레드가 출력하는 동안 다른 스레드는 대기
- `wait()`와 `notify()`도 스레드의 실행 순서를 조정하는 방법으로 소개됨

## 10. 실습에서 확인할 점
- `run()` 작성 → 스레드 객체 생성 → `start()` 호출의 순서
- 여러 스레드의 출력 순서가 고정되지 않는 이유
- `sleep()`을 추가했을 때 출력 과정의 변화
- 동기화 전에는 출력이 섞이고, 동기화 후에는 **하나의 출력 단위가 끝날 때까지 대기**하는 차이
- 스레드 실행을 종료할 때 `run()`이 끝나는 조건 확인

## 11. 핵심 정리
- **Thread 상속 / Runnable 구현** 두 가지 스레드 생성 방법 구분
- **start()**는 별도 스레드를 시작하고, **run()**은 수행할 작업을 정의
- 멀티스레드는 여러 실행 흐름이 함께 진행되므로 출력 순서가 달라질 수 있음
- 공유 자원에 동시에 접근할 때는 **synchronized**로 동기화 검토
