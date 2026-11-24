# JAVA프로그래밍응용 12주차 배운 내용 정리

## 1. 네트워크 프로그래밍의 개념
- **네트워크 프로그래밍** : 네트워크로 연결된 프로그램이 데이터를 주고받도록 만드는 것
- **서버(Server)** : 서비스를 제공하는 컴퓨터 또는 프로그램
- **클라이언트(Client)** : 서버에 서비스를 요청하여 이용하는 컴퓨터 또는 프로그램
- **프로토콜(Protocol)** : 서버와 클라이언트가 데이터를 교환하기 위해 따르는 통신 규칙
- **HTTP** : 웹서버와 웹브라우저 간 통신에 사용
- **SMTP** : 메일 전송에 사용

## 2. TCP/IP
- **TCP/IP** : 네트워크에서 데이터를 주고받기 위한 통신 프로토콜 체계
- **TCP** : 연결을 설정한 뒤 데이터를 전송하는 연결형 통신 방식
- TCP에서는 전송한 데이터가 순서대로 응용프로그램에 전달되도록 지원
- 웹(HTTP), 이메일, FTP 등의 통신에서 TCP가 활용됨
- 네트워크 통신은 응용프로그램, 전송, 네트워크, 링크 계층 등의 역할로 나누어 이해할 수 있음

## 3. IP 주소와 DNS
- **IP 주소** : 네트워크에서 통신 대상을 식별하기 위한 주소
- **IPv4** : 32비트 주소를 점(.)으로 구분된 네 숫자로 표현
- **IPv6** : 128비트 주소 체계
- **도메인 이름** : IP 주소를 대신하여 사용하는 사람이 읽기 쉬운 이름
- **DNS(Domain Name System)** : 도메인 이름을 IP 주소로 찾을 수 있게 하는 시스템
- **localhost** : 현재 자신의 컴퓨터를 가리키는 이름
- **ipconfig** : Windows에서 네트워크 설정과 IP 주소를 확인하는 명령

## 4. 포트(Port)
- **IP 주소** : 통신할 컴퓨터나 네트워크 인터페이스를 식별
- **포트 번호** : 컴퓨터 안에서 통신할 프로그램의 서비스 끝점을 식별
- **잘 알려진 포트(Well-known ports)** : 0~1023번
  - **FTP** : 21번
  - **SSH** : 22번
  - **HTTP** : 80번
- 서버에 접속할 때는 대상의 **IP 주소(또는 호스트명)와 포트 번호**를 함께 사용
- 실습에서는 채팅 서버의 포트로 **9999**를 사용

## 5. 소켓(Socket)
- **소켓** : 두 프로그램이 네트워크로 데이터를 주고받는 통신 연결의 끝점
- **소켓 프로그래밍** : 소켓과 입출력 스트림을 이용하여 네트워크 통신 프로그램 구현
- **java.net** 패키지에서 네트워크 관련 클래스 제공
- **Socket** : 클라이언트 연결과 연결된 상대방과의 통신에 사용
- **ServerSocket** : 서버에서 클라이언트의 연결 요청을 기다리고 받아들이는 데 사용

## 6. 클라이언트 소켓 생성
- `new Socket("localhost", 9999)` : 현재 컴퓨터의 9999번 포트에 접속
- `getInputStream()` : 서버로부터 들어오는 데이터를 읽을 스트림
- `getOutputStream()` : 서버로 데이터를 보낼 스트림
- `close()` : 사용한 소켓 닫기

```java
Socket socket = new Socket("localhost", 9999);

BufferedReader in = new BufferedReader(
    new InputStreamReader(socket.getInputStream()));
BufferedWriter out = new BufferedWriter(
    new OutputStreamWriter(socket.getOutputStream()));

out.write("hello\n");
out.flush();
String reply = in.readLine();

socket.close();
```

- **BufferedReader / BufferedWriter** : 문자열을 줄 단위로 받거나 버퍼를 이용해 전송
- **write()**로 데이터 기록 후 **flush()**로 버퍼의 내용을 전송
- **readLine()**은 줄바꿈 문자를 기준으로 한 줄을 읽음

## 7. 서버 소켓과 연결 수락
- **ServerSocket** : 지정한 포트에서 클라이언트의 연결 요청을 기다리는 클래스
- `new ServerSocket(9999)` : 9999번 포트로 접속받을 서버 소켓 생성
- `accept()` : 클라이언트의 요청을 기다렸다가 연결되면 통신용 `Socket` 반환
- 수락한 뒤에는 **반환된 Socket**을 이용해 해당 클라이언트와 데이터 교환

```java
ServerSocket server = new ServerSocket(9999);
Socket socket = server.accept();

BufferedReader in = new BufferedReader(
    new InputStreamReader(socket.getInputStream()));
BufferedWriter out = new BufferedWriter(
    new OutputStreamWriter(socket.getOutputStream()));
```

## 8. 서버와 클라이언트의 연결 과정
1. 서버가 `ServerSocket`을 생성하고 접속 요청 대기
2. 클라이언트가 서버의 주소와 포트로 `Socket` 생성
3. 서버의 `accept()`가 연결을 수락하고 통신용 `Socket` 반환
4. 양쪽에서 입출력 스트림을 생성하여 데이터 송수신
5. 통신이 끝나면 소켓과 스트림 등 자원 정리
- **서버 소켓**은 연결을 받아들이고, **연결된 소켓**은 실제 데이터를 주고받는 역할

## 9. 서버·클라이언트 1:1 채팅 실습
- **ServerTest.java** : 서버 소켓 생성 → 클라이언트 연결 대기 → 메시지 수신 → 답장 전송
- **ClientTest.java** : 서버에 연결 → 키보드 입력을 전송 → 서버의 답장 수신
- 서버와 클라이언트가 **한 번씩 번갈아 가며** 문자열을 전송하는 구조
- 전송 문자열 끝에 `"\n"`을 붙이고 `readLine()`으로 한 줄씩 읽음
- `Scanner(System.in)`으로 콘솔에서 채팅 메시지 입력
- `"bye"` 메시지가 전달되면 채팅을 종료하도록 구성

## 10. 채팅 프로그램의 주요 코드 흐름
- **서버** : `new ServerSocket(9999)` → `accept()` → `readLine()` → `write()` → `flush()`
- **클라이언트** : `new Socket("localhost", 9999)` → `nextLine()` → `write()` → `flush()` → `readLine()`
- 상대방의 메시지를 수신한 후 다음 메시지를 보내는 순서를 이해
- `IOException` 등 통신 중 발생할 수 있는 오류를 예외 처리
- 실행 시 **서버를 먼저 시작**한 다음 클라이언트를 실행하여 연결 테스트

## 11. 핵심 정리
- **서버는 서비스 제공 / 클라이언트는 서비스 요청**이라는 역할 구분
- **IP 주소는 통신 대상**, **포트는 대상 프로그램의 통신 끝점**을 지정
- **Socket**은 데이터 송수신, **ServerSocket**은 연결 요청 대기·수락에 사용
- **getInputStream() / getOutputStream()**으로 입출력 스트림 생성
- **write() → flush()**, **readLine()**의 흐름을 이해
- 9999번 포트를 이용한 **1:1 채팅 프로그램**에서 연결·송수신·종료 과정 복습
