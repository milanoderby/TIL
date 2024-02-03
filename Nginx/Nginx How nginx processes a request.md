# Nginx How nginx processes a request

https://nginx.org/en/docs/http/request_processing.html

<br>

## 이름 기반의 가상 서버로 request 전송

먼저, Nginx 는 어떤 server가 request를 처리해야하는지를 결정합니다.

```nginx
server {
    listen      80;
    server_name example.org www.example.org;
    ...
}

server {
    listen      80;
    server_name example.net www.example.net;
    ...
}

server {
    listen      80;
    server_name example.com www.example.com;
    ...
}
```

<br>

1. 모든 server 블록의 `listen 지시자 파라미터` 중에 `request의 port번호`와 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 server가 없다면, request가 어느 server로도 전송되지 않습니다.
   - 매칭되는 server가 있다면, 다음 과정을 수행하여 request를 전송할 server를 탐색합니다.
2. 모든 server 블록의 `server_name 지시자 파라미터` 중에 `request 의 Host 헤더 값`과 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 server가 있다면, 해당 서버로 request를 전송합니다.
   - 매칭되는 server가 없거나 request의 Host 헤더 자체가 없다면, `request의 port  번호를 listen 하는 서버 중 default server`로 request를 전송합니다.
   - `request의 port  번호를 listen 하는 서버 중 default server` 는 아래와 같은 방식으로 판단합니다.
     - `listen 지시자의 파라미터`에 `default_server`값이 명시된 server
     - 위에 해당하는 server가 없다면, `request의 port  번호를 listen 하는 서버` 중 가장 첫 번째 server

<br>

## 정의되지 않은 서버 이름으로 요청 처리를 방지하는 방법

`Host 헤더 값`이 없는 request를 허용되지 않아야한다면, 아래와 같이 server를 구성합니다.

```nginx
server {
    listen      80;
    server_name "";
    return      444;
}
```

`Host 헤더 값`이 없는 request는 위의 server로 전송될 것 이고, nginx 비표준 코드인 444 로 응답될 것입니다.

<br>

## 이름 기반 및 IP 기반 혼합 가상 서버

listen 지시자에 port 번호 외에 IP주소까지 명시한 예시 구성을 알아봅니다.

```nginx
# 1번 server
server {
    listen      192.168.1.1:80;
    server_name example.org www.example.org;
    ...
}

# 2번 server
server {
    listen      192.168.1.1:80;
    server_name example.net www.example.net;
    ...
}

# 3번 server
server {
    listen      192.168.1.2:80;
    server_name example.com www.example.com;
    ...
}
```

<br>

원리는 위에서 살펴본 **이름 기반의 가상 서버로 request 전송**과 동일합니다. 다만, 1번 과정에서 확인하는 값이 port번호 뿐만 아니라 IP주소도 추가된 것입니다.

1. 모든 server 블록의 `listen 지시자 파라미터` 중에 `request의 IP주소와 port번호`와 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 server가 없다면, request가 어느 server로도 전송되지 않습니다.
   - 매칭되는 server가 있다면, 다음 과정을 수행하여 request를 전송할 server를 탐색합니다.
2. 모든 server 블록의 `server_name 지시자 파라미터` 중에 `request의 Host 헤더 값`과 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 server가 있다면, 해당 서버로 request를 전송합니다.
   - 매칭되는 server가 없거나 request의 Host 헤더 자체가 없다면, `request의 port  번호를 listen 하는 서버 중 default server`로 request를 전송합니다.
   - `request의 port  번호를 listen 하는 서버 중 default server` 는 아래와 같은 방식으로 판단합니다.
     - `listen 지시자의 파라미터`에 `default_server`값이 명시된 server
     - 위에 해당하는 server가 없다면, `request의 port  번호를 listen 하는 서버` 중 가장 첫 번째 server

<br>

### 예시 - `curl -X GET '192.168.1.1:80' -H "Host: example.com "`

1. 모든 server 블록의 `listen 지시자 파라미터` 중에 `request의 IP주소와 port번호`와 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 `server 블록`이 2개 있으므로 2개 블록에 대해 아래 과정을 수행합니다.
2. 모든 server 블록의 `server_name 지시자 파라미터` 중에 `request의 Host 헤더 값`과 매칭되는 것이 있는지 확인합니다.
   - 매칭되는 server가 없기 때문에 `request의 port  번호를 listen 하는 서버 중 default server`로 request를 전송합니다.
   - `request의 port  번호를 listen 하는 서버 중 default server` 는 아래와 같은 방식으로 판단합니다.
     - `listen 지시자의 파라미터`에 `default_server`값이 명시된 server
     - 위에 해당하는 server가 없으므로 `request의 port  번호를 listen 하는 서버` 중 가장 첫 번째 server 블록인 `1번 server`로 request를 전송하게 됩니다.

