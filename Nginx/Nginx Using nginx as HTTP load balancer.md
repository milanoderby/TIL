## Nginx Using nginx as HTTP load balancer

https://nginx.org/en/docs/http/load_balancing.html

<br>

### 개요

로드밸런싱은 리소스 사용 최적화, 처리량 최대화, 지연 감소, 내결함성 보장을 위해 자주 사용되는 기술입니다. 

nginx는 여러 애플리케이션으로 트래픽을 분산하고, 성능, 확장성, 안정성을 향상시키기 위한 HTTP 로드밸런서의 역할을 수행할 수 있습니다.

<br>

### 로드밸런싱 방식

1. 라운드로빈
   - upstream에 명시된 애플리케이션 server에 순서대로 request를 전달합니다.
2. 최소커넥션
   - active connection의 개수가 가장 적은 server에 request를 전달합니다.
3. IP해싱
   - 해시 함수를 이용하여 request를 전송할 server를 선택합니다.

<br>

### 기본 로드밸런싱 구성 (라운드로빈 방식)

```nginx
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```

<br>

`upstream 블록` 에는 3개의 애플리케이션 server가 명시되어 있습니다.

`upstream 블록` 내에 `로드밸런싱 method`가 명시되어 있지 않다면 기본적으로 라운드로빈 방식을 사용합니다.

80포트로 들어오는 모든 요청은 `myapp1 upstream의 server그룹`으로 전송됩니다.

여기서는 HTTP로 프록시 하지만, HTTPS 같은 다른 프로토콜로 프록시도 가능합니다.

<br>

### 최소커넥션 로드밸런싱

또 다른 로드 밸런싱 방식은 최소 연결 우선 방식입니다. 

이 방식을 사용하면 `처리시간이 더 오래 걸리는 request`가 있을 때, 애플리케이션 인스턴스의 부하를 더 공정하게 나눠줄 수 있습니다.

이 방식을 사용하면 nginx는 과도한 request 처리로 바쁜 애플리케이션 server에 더 많은 부하를 주지 않습니다.

이 방식을 사용하기 위해 `upstream 블록` 내에 `least_conn 지시자`를 명시합니다.

```nginx
    upstream myapp1 {
        least_conn;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }
```

<br>

### 세션 지속을 위한 로드밸런싱 (IP해싱 방식)

라운드로빈 방식과 최소커넥션 방식은 동일한 client의 연속적인 request를 각각 다른 server로 전송할 가능성이 있습니다.

한 client의 request를 계속 동일한 server로 전송하고 싶다면 (즉, `session sticky`하게  또는 `session persistent` 한 방식으로 처리하고 싶다면) IP해싱 로드밸런싱 방법을 사용할 수 있습니다.

IP해싱 방식을 사용하면 `클라이언트의 IP 주소`를 `해싱 함수의 키`로 사용하여 `upstream 블록 내의 서버` 중 어떤 서버로 전송할지를 결정합니다.

이 방식을 사용하면 해당 서버를 사용할 수 없는 경우를 제외하고 동일한 클라이언트의 요청이 항상 동일한 서버로 전달됩니다.

이 방식을 사용하기 위해 `upstream 블록` 내에 `ip_hash 지시자`를 명시합니다.

```nginx
    upstream myapp1 {
        ip_hash;
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }
```

<br>

### 가중치를 부여한 로드밸런싱

server 가중치를 사용하여 로드밸런싱 알고리즘에 영향을 줄 수 있습니다.

지금까지 알아본 구성에서는 server 가중치를 부여하지 않았습니다. 즉, 명시된 모든 server가 로드밸런싱 방식에 대해 동일하게 부하를 받을 자격이 있습니다.

`server 지시자`에 `weight` 파라미터가 로드밸런싱을 결정할 때, 고려사항이 됩니다.

```nginx
    upstream myapp1 {
        server srv1.example.com weight=3;
        server srv2.example.com;
        server srv3.example.com;
    }
```

위와 같은 구성에서는 5번의 request가 들어오면 3번의 request는 srv1으로 전달되고, 1번의 request는 srv2으로 전달되고, 1번의 request는 srv3으로 전달될 것입니다.

<br>

### 헬스체크

nginx 에서 리버스 프록시 구현은 `in-band 또는 passive` 서버 헬스체크를 포함합니다.

만약, 특정 server의 응답이 실패하면 이 server를 실패로 표시하고 일정 시간동안 인입되는 요청들을 이 server로 전송하지 않습니다.

####  [max_fails](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server) 지시자

- `server 사용불가` 판정을 위해 `fail_timeout` 내에 연속적으로 발생해야하는 server 통신 실패 횟수를 명시합니다.
- 기본 값은 1입니다.
- 0으로 설정되어 있다면 해당 server에 대해 헬스체크를 수행하지 않습니다.
- `upstream 블록` 내에 `server 지시자`의 파라미터로 다음과 같이 명시합니다.
- `server srv2.example.com max_fails=3;`

<br>

#### [fail_timeout](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#server) 파라미터

- `server 사용불가` 판정을 위해 연속적으로 발생해야하는 server 통신 실패 횟수를 확인하는 기간을 명시합니다.
- `server 사용불가`  판정을 받은 후, `server 사용불가` 상태로 마킹하는 기간을 의미하기도 합니다.
- `server 사용불가` 판정 이후, `fail_timeout` 시간이 지나면 실제 client의 request를 `server 가용여부 확인`을 위해 전송합니다. 만약, 정찰 request가 성공한다면, 다시 `server 사용가능`상태로 변경합니다.
- 기본 값은 10초입니다.
- `upstream 블록` 내에 `server 지시자`의 파라미터로 다음과 같이 명시합니다.
- `server srv2.example.com fail_timeout=5s;`