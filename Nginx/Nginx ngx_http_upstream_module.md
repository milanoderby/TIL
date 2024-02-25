# Nginx ngx_http_upstream_module

https://nginx.org/en/docs/http/ngx_http_upstream_module.html

ngx_http_upstream_module 모듈은 [proxy_pass](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass), [fastcgi_pass](https://nginx.org/en/docs/http/ngx_http_fastcgi_module.html#fastcgi_pass), [uwsgi_pass](https://nginx.org/en/docs/http/ngx_http_uwsgi_module.html#uwsgi_pass), [scgi_pass](https://nginx.org/en/docs/http/ngx_http_scgi_module.html#scgi_pass), [memcached_pass](https://nginx.org/en/docs/http/ngx_http_memcached_module.html#memcached_pass), [grpc_pass](https://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass) 지시어에서 참조할 수 있는 서버 그룹을 정의하는 데 사용됩니다.

Nginx Reverse Proxy 구성 시, 사용합니다.

<br>

### Nginx Reverse Proxy 서버 그룹 구성 예시

- 설정 예시는 아래와 같습니다.

  ```nginx
  upstream backend {
      server backend1.example.com;
      server backend2.example.com;
  
      server backup1.example.com   backup;
      server backup2.example.com   backup;
  }
  
  server {
      location / {
          proxy_pass http://backend;
      }
  }
  ```

- 기본 설정 값 외에 다른 설정으로 변경이 필요할 때, docs를 참고합니다.

<br>

### [ngx_http_upstream_module모듈에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_upstream_module.html#variables)

- `$upstream_addr` : upstream 서버의 IP와 포트
- `$upstream_http_name` : upstream 서버가 전송한 응답 헤더 필드 `name` 의 값
- `$upstream_cookie_name` : upstream 서버가 전송한 응답 헤더 필드 `Set-Cookie` 에 채운 `name` 이름을 가지는 쿠키의 값입니다.
- `$upstream_status` : upstream 서버가 전송한 응답 코드
- `$upstream_response_time` : upstream 서버로부터 응답을 받는데까지 소요된 시간 (초 단위, 소수점으로 밀리초단위까지 계산하여 노출)

<br>