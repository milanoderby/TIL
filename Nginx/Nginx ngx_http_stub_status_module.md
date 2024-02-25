# Nginx ngx_http_stub_status_module

https://nginx.org/en/docs/http/ngx_http_stub_status_module.html

ngx_http_stub_status_module 모듈은 기본 상태 정보에 대한 접근을 제공합니다.

위 모듈을 통해 얻어낸 정보를 프로메테우스로 전송하여 모니터링하는 방식으로 사용됩니다.

<br>

### [`stub_status` 지시어](https://nginx.org/en/docs/http/ngx_http_stub_status_module.html#stub_status)

```nginx
stub_status;
```

- 기본 상태 정보는`stub_status 지시어를 감싸고 있는 location 블록`을 통해 제공됩니다.

- 예시는 아래와 같습니다.

  ```nginx
  location = /basic_status {
      stub_status;
  }
  ```
  
- 기본 상태 정보가 포함된 간단한 웹 페이지를 생성합니다.

  ```
  Active connections: 291
  server accepts handled requests
   16630948 16630948 31070465
  Reading: 6 Writing: 179 Waiting: 106
  ```

  - `Active connections` : `Waiting Connections`를 포함한 현재 활성 클라이언트 연결 수입니다.
  - `Reading` : nginx가 요청 헤더를 읽는 현재 연결 수입니다.
  - `Writing` : nginx가 클라이언트에 응답을 쓰는 현재 연결 수입니다.
  - `Waiting` : 요청을 기다리고 있는 현재 유휴 클라이언트 연결 수입니다.
  - `accepts` : 수락된 총 연결 수입니다.
  - `handled` : 처리된 총 연결 수입니다. 일반적으로 이 값은 일부 리소스 제한(예시, [worker_connections](https://nginx.org/en/docs/ngx_core_module.html#worker_connections) 제한)에 도달하지 않는 한 accepts와 동일합니다.
  - `requests` : 총 요청 수입니다.

- `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [ngx_http_stub_status_module 에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_stub_status_module.html#variables)

자세한 사항은 docs에서 확인합니다.