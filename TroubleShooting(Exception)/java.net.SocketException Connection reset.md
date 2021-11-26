# java.net.SocketException: Connection reset

### 문제

local 환경에서는 정상동작했으나, server 환경에서 정상동작하지 않았고, Exception의 원인을 찾아보니 위의 Exception이 발생하였습니다.

<br>

### 구글링 - 원인파악

다양한 원인으로 문제가 발생할 수 있음을 확인하였습니다.

또한, 얼핏 예외 로그만 보시면 Server 문제 같아보이지만, Client 측 문제임을 알 수 있었습니다.

Server가 Connection 자체를 거부하는 경우, **Connection reset by peer** 라는 예외 메세지가 발생한다고 합니다.

#### 가능성 있는 원인

1. TLS 버전문제 (v1.2를 사용하지 않을 때, 발생)
2. Connection을 열고, 사용 후에 닫지 않은 경우 발생 (finally 문에서 connection.close 처리 필요)
3. 소켓 연결을 이르게 닫는 경우, 발생 (connection timeout, socket timeout 시간 연장 필요)

<br>

### 결론

local 환경에서는 TLS v1.2가 기본설정인 Java8을 사용하였고,

server 환경에서는 TLS v1.0이 기본설정인 java7을 사용하여 발생한 문제로 의심됩니다. (문제해결 진행 중)

<br>

### 참고자료

https://itstein.tistory.com/15

https://96-brain.tistory.com/76

https://multifrontgarden.tistory.com/46