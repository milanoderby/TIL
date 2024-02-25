# Nginx ngx_http_access_module

https://nginx.org/en/docs/http/ngx_http_access_module.html

ngx_http_access_module 모듈을 사용하면 특정 클라이언트 주소에 대한 액세스를 제한할 수 있습니다.

<br>

### [`allow` 지시어](https://nginx.org/en/docs/http/ngx_http_access_module.html#allow) 와 [`deny` 지시어](https://nginx.org/en/docs/http/ngx_http_access_module.html#deny)

```nginx
allow address | CIDR | unix: | all;
deny address | CIDR | unix: | all;
```

- client IP 주소 기반의 접근 제한을 명시하는 명령어입니다. 

- `address` 는 접근을 허용할 client IP 주소를 의미합니다.

- `CIDR` 는 접근을 허용할 client IP 주소 대역을 의미합니다.

- `unix` 로 지정하면 모든 UNIX 도메인 소켓에 대한 접근을 허용합니다.

- `all` 로 지정하면 모든 접근을 허용합니다.

- 예시는 아래와 같습니다.

  ```nginx
  location / {
      deny  192.168.1.1;
      allow 192.168.1.0/24;
      allow 10.1.1.0/16;
      deny  all;
  }
  ```

- 접근 제한 규칙은 client IP 주소와 매칭되는 항목이 발견될 때까지 순서대로 확인합니다.

- 위의 예시 설정은 아래와 같이 접근 제한을 수행합니다.

- `192.168.1.1` 을 제외한 IPv4 네트워크 `192.168.1.0/24`, `10.1.1.0/1` 에서만 접근이 허용됩니다.

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>