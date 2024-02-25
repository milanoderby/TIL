# Nginx ngx_http_proxy_module

https://nginx.org/en/docs/http/ngx_http_proxy_module.html

ngx_http_proxy_module 모듈을 사용하면 요청을 다른 서버로 전달할 수 있습니다.

Nginx Reverse Proxy 구성 시, 사용합니다.

<br>

### Nginx Reverse Proxy를 사용하여 얻는 효과

* `Security`: WAS는 DB에 직접 접근하는 서버이기 때문에, 외부에 노출되면, 시스템이 공격에 취약해집니다. 그렇기 때문에, 웹 서버를 가장 외부 단에 두어 시스템을 보호하는 역할을 수행하기 위함입니다.
* `Load Balancing`: 클라이언트의 Request로 발생하는 부하(traffic)를 여러 대의 WAS에 적절하게 분산하기 위함입니다.
* `Web Acceleration`: WAS까지 갈 필요 없는 정적자원 요청을 웹서버 단에서 처리하거나, 캐싱하여 성능을 향상하기 위함입니다.
* 참조 - [https://www.nginx.com/resources/glossary/reverse-proxy-server/](https://www.nginx.com/resources/glossary/reverse-proxy-server/)

<br>

### [`proxy_bind` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_bind)

```nginx
proxy_bind address [transparent] | off;
```

- 지정된 `로컬 IP 주소 및 포트`에서 프록시 서버로 나가는 connection을 만듭니다.

- `address` 는 `로컬 IP 주소 및 포트` 를 의미합니다.

- `address 지시어` 뒤에 `transparent 지시어` 를 설정하면 `client 실제 IP 주소와 같은 로컬이 아닌 IP 주소`에서 프록시 서버로의 나가는 connection을 허용합니다.

  - 예시는 아래와 같습니다.

    ```nginx
    proxy_bind $remote_addr transparent;
    ```

- `off` 로 설정하면 이전 컨텍스트에서 상속된 `proxy_bind` 지시어의 효과를 취소하여 시스템이 로컬 IP 주소와 포트를 자동 할당할 수 있도록 합니다.

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_buffering` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffering)

```nginx
proxy_buffering on | off;
```

- 프록시 서버로부터 받은 응답에 대한 버퍼링을 활성화하거나 비활성화합니다.
- 버퍼링이 활성화되면 nginx는 가능한 빨리 프록시 서버로부터 응답을 수신하여 이를 버퍼에 저장합니다. 이 때, 버퍼의 크기는 `proxy_buffer_size 지시어` 와 `proxy_buffers 지시어`에 의해 결정됩니다.
  - 전체 응답이 메모리에 맞지 않으면 응답의 일부를 디스크의 임시 파일에 저장할 수 있습니다. 임시 파일의 경로는 [`proxy_temp_path` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_path)에 의해 결정됩니다.
  - **임시 파일에 쓰기**는 [`proxy_max_temp_file_size 지시어`](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_max_temp_file_size) 와 [`proxy_temp_file_write_size 지시어`](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_temp_file_write_size) 에 의해 제어됩니다.
- 버퍼링이 비활성화되면 nginx는 프록시 서버로 받은 응답을 받자 마자 비동기적으로 client 측에 전달합니다. 이 때, nginx가 프록시 서버로부터 한 번에 수신할 수 있는 데이터의 최대 크기는 `proxy_buffer_size 지시어` 에 의해 설정됩니다.
- 기본 설정 값은 `proxy_buffering on;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_buffer_size` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)

```nginx
proxy_buffer_size size;
```

- 프록시 서버로부터 받은 응답의 첫 번째 부분을 읽는 데 사용되는 버퍼의 크기를 설정합니다. 일반적으로 응답의 첫 번째 부분은 크기가 작은 응답 헤더가 포함됩니다.
- 기본적으로 버퍼 크기는 메모리 페이지 1개로 설정됩니다. OS에 따라 4K 또는 8K입니다.
- `size` 는 `응답의 첫 번째 부분을 읽는 데 사용되는 버퍼의 크기` 를 의미합니다.
- 기본 설정 값은 `proxy_buffer_size 4k|8k;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_buffers` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffers)

```nginx
proxy_buffers number size;
```

- 하나의 connection에 대해 프록시 서버에서 응답을 읽는 데 사용되는 버퍼의 수와 크기를 설정합니다.
- `proxy_buffer_size 지시어` 는 응답의 첫 번째 부분에만 적용되는 설정이고, `proxy_buffers 지시어` 는 응답의 나머지 부분 전체에 적용되는 설정입니다.
- 기본적으로 버퍼 크기는 메모리 페이지 1개로 설정됩니다. OS에 따라 4K 또는 8K입니다.
- `number` 는 `응답을 읽는 데 사용되는 버퍼의 개수` 를 의미합니다.
- `size` 는 `응답을 읽는 데 사용되는 버퍼의 크기` 를 의미합니다.
- 기본 설정 값은 `proxy_buffers 8 4k|8k;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_busy_buffers_size` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_busy_buffers_size)

```nginx
proxy_busy_buffers_size size;
```

- 프록시 서버의 응답 버퍼링이 활성화되면 응답이 아직 완전히 읽혀지지 않은 동안 **client에 응답을 전송하는 데 사용될 수 있는 버퍼의 총 크기를 제한**합니다. 그 동안 나머지 버퍼들은 응답을 읽는 데 사용될 수 있으며, 필요한 경우 응답의 일부를 임시 파일에 버퍼링하는 데 사용될 수 있습니다.
- 즉, 전체 버퍼를 전송하는 데에만 사용하면 안되므로 이러한 제한을 두는 것입니다.
- `size` 는 `client에 응답을 전송하는 데 사용될 수 있는 버퍼의 총 크기` 를 의미합니다.
- 기본적으로 `size` 는 `proxy_buffer_size 지시어` 와 `proxy_buffers 지시어` 에 의해 설정된 두 버퍼의 크기로 제한됩니다.
- 기본 설정 값은 `proxy_busy_buffers_size 8k|16k;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_http_version` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_http_version)

```nginx
proxy_http_version 1.0 | 1.1;
```

- 프록시를 위한 HTTP 프로토콜 버전을 설정합니다.
- Keepalive 연결 및 NTLM 인증기능을 사용하기 위해서 버전 **1.1을 사용하는 것이 좋습니다.**
- 기본 설정 값은 `proxy_http_version 1.0;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_pass` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass)

```nginx
 proxy_pass URL;
```

- 프록시 서버의 `프로토콜` 및 `address`를 설정합니다. 추가적으로 `URI`와 같은 `location`을 명시할 수도 있습니다.

- `프로토콜` 은 `http` 또는 `https` 로 설정할 수 있습니다.

- `address` 는 `도메인 명` 또는 `IP 주소 (포트까지 명시할 수도 있음)` 로 설정할 수 있습니다.

  - `도메인 명`이 여러 개의 주소로 맵핑이 되면 모든 주소가 라운드 로빈 방식에 의해 사용됩니다.
  - `도메인 명` 이 `upstream 지시어`에 설정된 `도메인 명` (여러 개의 서버를 묶은 서버 그룹)일 수도 있습니다.
  - `도메인 명` 으로 지정되어 있으면 `upstream 지시어` 들 중 일치하는 이름을 검색합니다. `upstream 지시어` 의 값과 일치하는 `도메인 명`이 없는 경우 `resolver 지시어`를 사용하여 검색합니다.

- `URI` 가 전달되는 방식은 `location`의 구성과 `proxy_pass`가 `URI`를 포함하느냐에 따라 복잡합니다.

  - 가장 간단한 방식만 알아봅니다. 그 외의 방식은 필요할 때, docs를 참고합시다.
  - `proxy_pass 지시어`의 파라미터에 `URI` 를 포함하지 않으면 프록시 서버로 `request URI`를 `client`측 에서 보낸 것과 동일한 형식으로 전달합니다.

- 예시는 아래와 같습니다.

  ```nginx
  location /some/path/ {
      proxy_pass http://127.0.0.1;
  }
  ```

<br>

### [`proxy_hide_header ` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_hide_header) 와 [`proxy_pass_header` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_header)

```nginx
proxy_hide_header field;
proxy_pass_header field;
```

- 기본적으로 nginx는 "Date", "Server", "X-Pad" 및 "X-Accel-..." 같은 `프록시 서버의 응답의 헤더 필드` 를 `client` 측으로 전달하지 않습니다. 
- `proxy_hide_header` 지시어는 `client` 측에 전달하지 않을 헤더 필드들을 추가로 설정합니다.
- `proxy_pass_header` 지시어는 `client` 측으로 전달을 허용할 헤더 필드들을 설정합니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_pass_request_headers ` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_headers) 와 [`proxy_pass_request_body` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass_request_body)

```nginx
proxy_pass_request_headers on | off;
proxy_pass_request_body on | off;
```

- `proxy_pass_request_headers 지시어` 는 원래 `request 의 헤더 필드`를 `프록시 서버`로 전달할지 여부를 결정합니다.
  - 기본 설정 값은 `proxy_pass_request_headers on;` 입니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.
- `proxy_pass_request_body 지시어` 는 원래 `request 의 body`를 `프록시 서버`로 전달할지 여부를 결정합니다.
  - 기본 설정 값은 `proxy_pass_request_body on;` 입니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_set_header` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header) 와 [`proxy_set_body` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_body)

```nginx
proxy_set_header field value;
proxy_set_body value;
```

- `proxy_set_header 지시어` 는 프록시 서버로 전달할 request 헤더 필드를 재정의하거나 추가할 수 있습니다.
  - `field` 는 프록시 서버로 전달할 `헤더 필드의 이름` 을 의미합니다.
  - `value` 는 프록시 서버로 전달할 `헤더 필드의 값` 을 의미합니다. `value` 값이 `""`와 같은 빈 문자열로 설정되면 프록시 서버로 헤더 필드가 전달되지 않습니다.
  - 기본 설정 값은 `proxy_set_header Host $proxy_host; proxy_set_header Connection close;` 입니다.
  - `request의 Host 헤더 필드`를 `$proxy_host` 값이 아닌 `원본 request의 Host 헤더 필드 값`을 사용하고 싶다면 다음과 같이 설정할 수 있습니다. `proxy_set_header Host $http_host;`
  - `proxy_set_header 지시어` 가 없는 경우에만 이전 컨텍스트에 정의된 `proxy_set_header` 설정을 상속합니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.
- `proxy_set_body 지시어` 는 프록시 서버로 전달할 body를 재정의합니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_ignore_client_abort` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ignore_client_abort)

```nginx
proxy_ignore_client_abort on | off;
```

- `client`가 응답을 기다리지 않고 connection을 닫을 때, 프록시 서버와의 연결을 닫아야 하는지 여부를 결정합니다.
- `off`로 설정되어 있으면  `client`가 connection을 종료하면 nginx는 백엔드 서버로의 프록시 요청을 중단하여 불필요한 작업을 수행하지 않도록 합니다.
- 기본 설정 값은 `proxy_ignore_client_abort off;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_intercept_errors ` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_intercept_errors)

```nginx
proxy_intercept_errors on | off;
```

- 응답코드가 300 이상일 때, 프록시 서버의 응답을 `client` 측에 전달해야 하는지 아니면 `error_page 지시어`로 처리하기 위해 응답을 가로채 nginx로 리다이렉트해야 하는지 결정합니다.
- `off`로 설정되어 있으면 프록시 서버의 응답코드가 `300 이상` 이어도 응답을 그대로 `client` 측에 전달합니다.
- 기본 설정 값은 `proxy_intercept_errors off;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_limit_rate ` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_limit_rate)

```nginx
proxy_limit_rate rate;
```

- 프록시 서버에서 `응답을 읽는 속도`를 제한합니다.
- `rate` 는 제한을 걸어둘 `응답을 읽는 속도`를 의미합니다. 초당 바이트(byte per second) 단위로 설정합니다.
- `proxy_buffering 지시어`  가 `on` 으로 설정되어 있을 때에만 동작합니다.
- 하나의 `connection` 별로 속도 제한 기능이 적용되므로 `client` 측에서 2개의 connection을 사용한다면 전체 속도는 제한된 속도의 2배가 될 것입니다.
- `0` 으로 설정하면 속도 제한 기능을 비활성화합니다.
- 기본 설정 값은 `proxy_limit_rate 0;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_next_upstream ` 관련 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_next_upstream)

```nginx
proxy_next_upstream error | timeout | invalid_header | http_500 | http_502 | http_503 | http_504 | http_403 | http_404 | http_429 | non_idempotent | off ...;
proxy_next_upstream_timeout time;
proxy_next_upstream_tries number;
```

- `proxy_next_upstream` 지시어

  - `이미 프록시 서버로 전송된 request`가 `upstream 서버군에 정의된 다음 서버`로 전달되어야 하는 경우를 지정합니다.
  - `error` 로 설정하면 서버와의 연결하는 중, 서버에 요청을 전달하는 중, 응답 헤더를 읽는 중에 오류가 발생할 경우, `upstream 서버군에 정의된 다음 서버` 로 재전송합니다.

  - `timeout` 로 설정하면 서버와의 연결하는 중, 서버에 요청을 전달하는 중, 응답 헤더를 읽는 중에 타임아웃이 발생할 경우, `upstream 서버군에 정의된 다음 서버` 로 재전송합니다.

  - `invalid_header` 로 설정하면 서버가 비어 있거나 잘못된 응답을 반환한 경우, `upstream 서버군에 정의된 다음 서버` 로 재전송합니다.

  - `http_응답코드`형태로 설정하면 `서버가 해당하는 응답코드`를 반환한 경우, `upstream 서버군에 정의된 다음 서버` 로 재전송합니다.

  - `off` 로 설정하면  `upstream 서버군에 정의된 다음 서버` 로 재전송을 하지 않습니다.

  - 기본 설정 값은 `proxy_next_upstream error timeout;` 입니다.

  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

- `proxy_next_upstream_timeout` 지시어

  - request가 다음 서버로 전달될 수 있는 시간을 제한합니다. 
  - `0`으로 설정하면 시간 제한을 없앱니다.
  - 기본 설정 값은 `proxy_next_upstream_timeout 0;` 입니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

- `proxy_next_upstream_tries` 지시어

  - request가 다음 서버로 전달하는 시도 횟수를 제한합니다. 
  - `0`으로 설정하면 시도 횟수 제한을 없앱니다.
  - 기본 설정 값은 `proxy_next_upstream_tries 0;` 입니다.
  - `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`proxy_cache` 관련 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache)

```nginx
proxy_cache zone | off;
proxy_cache_background_update on | off;
proxy_cache_bypass string ...;
proxy_cache_convert_head on | off;
proxy_cache_key string;
proxy_cache_lock on | off;
proxy_cache_lock_age time;
proxy_cache_lock_timeout time;
proxy_cache_max_range_offset number;
proxy_cache_methods GET | HEAD | POST ...;
proxy_cache_min_uses number;
proxy_cache_path path [levels=levels] [use_temp_path=on|off] keys_zone=name:size [inactive=time] [max_size=size] [min_free=size] [manager_files=number] [manager_sleep=time] [manager_threshold=time] [loader_files=number] [loader_sleep=time] [loader_threshold=time] [purger=on|off] [purger_files=number] [purger_sleep=time] [purger_threshold=time];
proxy_cache_purge string ...;
proxy_cache_revalidate on | off;
proxy_cache_use_stale error | timeout | invalid_header | updating | http_500 | http_502 | http_503 | http_504 | http_403 | http_404 | http_429 | off ...;
proxy_cache_valid [code ...] time;
proxy_no_cache string ...;
```

- 기본적으로 `proxy_cache 관련 지시어`는 proxy server에서 받은 응답을 캐싱할 때, 사용하는 지시어들입니다.
- `proxy_cache 지시어` 의 기본 설정 값은 `proxy_cache off;` 이므로 활성화되지 않은 상태입니다.
- 필요에 의해 위 기능을 사용할 때, docs를 참고합시다.

<br>

### [`proxy_cookie` 관련 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_domain)

```nginx
proxy_cookie_domain off;
proxy_cookie_flags off | cookie [flag ...];
proxy_cookie_path off | path replacement;
```

- 기본적으로 `proxy_cookie 관련 지시어`는 proxy server에서 받은 응답의 `Set-Cookie 헤더 필드 값` 을 변경할 때, 사용하는 지시어들입니다.

- 위 지시어들의 기본 설정 값은 `off;` 이므로 활성화되지 않은 상태입니다.

- 필요에 의해 위 기능을 사용할 때, docs를 참고합시다.

<br>

### [`proxy_ssl` 관련 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_ssl_certificate)

```nginx
proxy_ssl_certificate_key file;
proxy_ssl_ciphers ciphers;
proxy_ssl_conf_command name value;
proxy_ssl_crl file;
proxy_ssl_name name;
proxy_ssl_password_file file;
proxy_ssl_protocols [SSLv2] [SSLv3] [TLSv1] [TLSv1.1] [TLSv1.2] [TLSv1.3];
proxy_ssl_server_name on | off;
proxy_ssl_session_reuse on | off;
proxy_ssl_trusted_certificate file;
proxy_ssl_verify on | off;
proxy_ssl_verify_depth number;
```

- 기본적으로 `proxy_ssl 관련 지시어`는 프록시된 HTTPS 서버에 대한 인증과 관련된 지시어들입니다.

- 필요에 의해 위 기능을 사용할 때, docs를 참고합시다.

<br>

### [ngx_http_proxy_module 모듈에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#variables)

- `$proxy_host` : `proxy_pass 지시어` 에 지정된 프록시 서버의 이름과 포트
- `$proxy_port` : `proxy_pass 지시어` 에 지정된 프록시 서버의 포트
- `proxy_add_x_forwarded_for` : client 측에서 전송한 request의 `X-Forwarded-For 헤더 필드` 의 뒤에 `,`와 `$remote_addr` 변수를 추가한 값

<br>

### 예시 설정 - /etc/nginx/conf.d/proxy.conf 

```nginx
location / {
    proxy_pass       http://localhost:8000;
    proxy_set_header Host      $http_host; # 아래 설정 또는 이 설정으로 적용
    proxy_set_header Host      $host; # 위 설정 또는 이 설정으로 적용
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto http;
    proxy_set_header Content-Type $content_type;
}
```

* proxy 서버로 요청을 전달할 때, 기존 요청 헤더 값도 전달해주어야 프록시 서버가 정상동작합니다.
  * 위와 같은 설정이 없다면, 요청을 처리하고 응답해줄 client를 찾을 수 없습니다. 또한, 각 request 의 헤더 필드 값을 못받아 정상적인 요청처리가 불가합니다.
  * 위와 같은 `proxy.conf` 파일을 생성 및 저장합니다.
* `nginx.conf` 파일에서 `include 지시어`를 이용하여 `proxy.conf` 파일의 구성을 포함하도록 합니다.

```nginx
include  /etc/nginx/conf.d/proxy.conf;
```

<br>