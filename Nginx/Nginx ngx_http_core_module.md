# Nginx ngx_http_core_module

https://nginx.org/en/docs/http/ngx_http_core_module.html

nginx 의 http 컨텍스트에서 정의되는 핵심 지시어와 관련하여 설명합니다.

<br>

### [`resolver` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver)

```nginx
resolver address ... [valid=time] [ipv4=on|off] [ipv6=on|off] [status_zone=zone];
```

- `upstream` 서버의 이름을 IP 주소로 해석하는 데 사용되는 `네임서버`를 설정합니다.

- 예시는 아래와 같습니다.

  ```nginx
  resolver 127.0.0.1 [::1]:5353;
  ```

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`default_type` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#default_type)

- 응답의 기본 MIME 타입을 정의합니다.
- MIME 타입에 대한 파일 이름 확장자의 매핑은 [`types 지시어`](https://nginx.org/en/docs/http/ngx_http_core_module.html#types)를 사용하여 설정할 수 있습니다.
- 기본 설정 값은 `default_type text/plain;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`server_tokens` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens)

```nginx
server_tokens on | off | build | string;
```

* Nginx의 기본 오류페이지와 응답 헤더의 `Server`값에서 nginx 버전을 보여줄지 말지를 설정합니다.
* 보안을 위해 Nginx 버전은 사용자에게 노출되지 않는 것이 좋으므로 `server_tokens off;`로 설정합니다.
* 기본 설정 값은 `server_tokens on;`입니다.

<br>

### [`sendfile` 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile)

```nginx
sendfile on;
```

* 서버에서 커널함수인 `sendfile()` 함수 사용을 허용할지 말지 설정합니다.
* 일반적으로 파일을 전송할 때, `read()`, `write()` 과정을 거칩니다. 동일한 파일객체에 대해서 2번의 I/O 작업으로 context switch가 발생하는 것은 비효율적입니다. 앞의 2번의 system call 대신에, `sendfile()` 함수를 사용하면, 전송속도를 최적화할 수 있습니다. 복사 붙여넣기가 아니라 파일 캐시의 주소 값(pointer)을 소켓으로 바로 전송하는 원리입니다. (참조: [https://www.netguru.com/codestories/nginx-tutorial-performance](https://www.netguru.com/codestories/nginx-tutorial-performance)
* `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.
* 기본 설정 값은 `sendfile off;` 입니다.

<br>

### [`limit_rate` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate)

```nginx
limit_rate rate;
```

- client에 응답 전송속도를 제한하는 기능입니다.
- `rate` 는 `초당 전송 byte (bps)`를 의미합니다. 가질 수 있는 값으로 `4k`, `1m`, `1g` 등이 있습니다.
- `rate` 값이 0이면 전송속도를 제한하지 않습니다.
- `하나의 request 별`로 제한이 적용됩니다. 즉, client가 2개의 connection을 이용할 경우 전체 전송속도는 2배가 됩니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.
- 기본 설정 값은 `limit_rate 0;` 입니다.

<br>

### [`limit_rate_after` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#limit_rate_after)

```nginx
limit_rate_after size;
```

- client에 응답 전송속도를 제한하기 시작하는 응답 전송량 임계치를 지정합니다.
- 응답 전송량이 `size` 크기보다 커지면, `limit_rate` 설정이 동작합니다.
- `size` 가 가질 수 있는 값으로 `4k`, `1m`, `1g` 등이 있습니다.
- `하나의 request 별`로 제한이 적용됩니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.
- 기본 설정 값은 `limit_rate_after 0;` 입니다.

<br>

### [`keepalive` 관련 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html)

```nginx
keepalive_disable none | browser ...;
keepalive_requests number;
keepalive_time time;
keepalive_timeout timeout [header_timeout];
```

* keep-alive 란?
  * HTTP Connection은 `connectionless`(비연결성) 특성을 가지고 있습니다. 클라이언트 요청 시마다 매번 connection을 맺고, 응답을 완료하면, 연결을 끊어버립니다.
  * 위와 같은 특성으로 client가 매번 새로운 `연결/해제` 과정에서 오버헤드가 발생합니다.
  * 이러한 오버헤드를 줄이기 위해 `연결을 재사용하도록 유지하는 방식`이 keep-alive입니다.
  * keep-alive를 통해 client와 server의 connection을 유지한다는 것은 서버의 메모리 자원 또한 해제되지 않고, 유지됨을 의미합니다.
  * 그래서, `재사용횟수 제한`, `유지시간 제한`, `서버자원 제한`을 두어 keep-alive connection들을 주기적으로 해제합니다.
* `keepalive_disable none | browser ...;`
  * 잘못 작동하는 브라우저와의 연결 유지를 비활성화합니다.
  * 브라우저 매개변수는 영향을 받는 브라우저를 지정합니다.
  * `msie6` 값으로 설정하면 POST 요청이 수신될 때, 오래된 버전의 MSIE와의 `keep-alive`를 비활성화합니다. 
  * `safari` 값으로 설정하면 macOS 및 macOS 유사 운영 체제에서 Safari 및 Safari 유사 브라우저와의 `keep-alive`를 비활성화합니다. 
  * `none` 값은 모든 브라우저와의 `keep-alive`를 활성화합니다.
  * 기본 설정 값은 `keepalive_disable msie6;` 입니다.

* `keepalive_requests`
  * 하나의 keep-alive connection 을 통해 처리할 수 있는 최대 요청 수를 설정합니다. (`재사용횟수 제한`)
  * 최대 요청 수를 초과하면 connection이 닫힙니다.
  * connection 당 메모리 할당을 해제하려면 주기적으로 연결을 닫는 것이 필요합니다. 따라서, 이 값을 너무 크게 설정하면 과도한 메모리 사용량이 발생할 수 있으므로 권장되지 않습니다.
  * 기본 설정 값은 `keepalive_requests 1000;` 입니다. 

* `keepalive_time`
  * 하나의 keepalive connection을 통해 요청을 처리할 수 있는 최대 시간을 제한합니다. 
  * 이 시간에 도달하면 후속 요청 처리 후에 연결이 닫힙니다.
  * 기본 설정 값은 `keepalive_time 1h;` 입니다.

* `keepalive_timeout`
  * 첫 번째 매개변수 `timeout`은 연결 유지 클라이언트 연결이 서버 측에서 열린 상태로 유지되는 시간 제한을 설정합니다.
  * 하나의 keep-alive connection을 몇 초간 유지할지 설정합니다. (`유지시간 제한`)
  * 두 번째 매개변수 `header_timeout`은 필수 값이 아니며, `Keep-Alive: timeout=time 응답 헤더 필드 값`  을 설정하는 용도입니다. 이는 실제 서버에 설정된 값과는 다를 수 있습니다.
  * 너무 길게 유지하면, 서버자원(메모리)의 한계점까지 connection이 맺어져있을 경우, `keep-alive connection`으로 인해 새로운 request가 거부될 수 있습니다.
  * 기본 설정 값은 `keepalive_timeout 75s;` 입니다.

* `upstream 컨텍스트`에서 설정가능한 keepalive 옵션을 따로 있습니다. - [참고자료](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)
  * `keepalive connections`
  * Nginx 서버에서 최대로 유지할 Connection의 수를 설정합니다. 이 수를 넘어가면, LRU(Least Recently Used) 알고리즘에 의해 가장 오래 전에 맺어진 connection 부터 제거합니다. (`서버자원 제한`)

<br>

### [`root` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#root)

```nginx
root path;
```

- `path`는 request 의 루트 디렉터리를 의미합니다. 예를 들어, 아래와 같은 설정이 있다고 합시다.

  ```nginx
  location /i/ {
      root /data/w3;
  }
  ```

- request 의 URI가 `/i/top.gif` 이면, `/data/w3/i/top.gif` 파일을 찾아 응답으로 반환할 것 입니다.

- 즉, `root 지시어에서 지정한 루트 디렉토리 경로` + `request의 URI`가 응답할 파일의 경로가 됩니다.

- 만약, `request의 URI` 를 합성하면, 적절한 파일 경로가 되지 않아서 변경해야할 경우, `alias 지시어`를 사용해야 합니다.

- 현재 컨텍스트에서 `root` 구문이 없을 경우, 상위 레벨의 컨텍스트 설정을 상속하여 사용합니다.

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`alias` 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html#alias)

```nginx
alias path;
```

- `path`는 request 의 루트 디렉터리를 의미합니다. 예를 들어, 아래와 같은 설정이 있다고 합시다.

  ```nginx
  location /i/ {
      root /data/w3;
  }
  ```

- request 의 URI가 `/i/top.gif` 이면, `/data/w3/top.gif` 파일을 찾아 응답으로 반환할 것 입니다.

- 즉, `root 지시어에서 지정한 루트 디렉토리 경로` + `request의 URI 에서 경로를 제외한 파일명`가 응답할 파일의 경로가 됩니다.

- 정규표현식으로 정의된 `location` 블록 안에서 `alias 지시어` 가 사용되는 경우, `location 지시어`의 정규표현식에는 `캡처, ()`가 포함되어야 하며 `alias 지시어`는 이렇게 캡처된 구문을 사용해야 합니다.

  ``` nginx
  location ~ ^/users/(.+\.(?:gif|jpe?g|png))$ {
      alias /data/w3/images/$1;
  }
  ```

- `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`satisfy` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#satisfy)

```nginx
satisfy all | any;
```

- `satisfy all;` 로 명시하면 모든 `접근 제한 기능 모듈` 을 모두 만족할 경우에만 접근을 허용합니다.

- `satisfy any;` 로 명시하면 `접근 제한 기능 모듈` 에서 정의한 접근 제한 중 하나라도 만족할 경우에 접근을 허용합니다.

- 예시는 아래와 같습니다.

  ``` nginx
  location / {
      satisfy any;
  
      allow 192.168.1.0/32;
      deny  all;
  
      auth_basic           "closed site";
      auth_basic_user_file conf/htpasswd;
  }
  ```

- 기본 설정 값은 `satisfy all;` 입니다.

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`try_files` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#try_files)

```nginx
try_files file ... uri;
try_files file ... =code;
```

- 지정된 순서대로 파일이 있는지 확인하고, 가장 먼저 발견된 파일을 요청 처리에 사용하는 지시어입니다.

- `index 지시어` 는 `request의 URI` 가 `/`로 끝날 때, 적용이 되기 때문에 적용하기가 어려운 반면, `try_files 지시어` 는 그러한 조건이 없기 때문에 비교적 사용하기 쉽습니다.

- `file ...` 는 응답에 사용할 파일을 지정합니다.

- `uri` 는 위에서 지정한 파일들이 발견되지 않으면 내부 redirect 를 보낼 `URI` 입니다. 예시는 아래와 같습니다.

  ```nginx
  location /images/ {
      try_files $uri /images/default.gif;
  }
  
  location = /images/default.gif {
      expires 30s;
  }
  ```

  - 위의 예시에서 `URI 경로의 파일` 을 탐색하여 응답에 사용합니다.
  - 만약, `URI 경로의 파일`이 없다면 `/images/default.gif` 경로로 내부 redirect를 수행합니다.
  - `/images/default.gif` URI에서 `expires 30s` 명령어를 수행합니다.

- `code` 는 위에서 지정한 파일들이 발견되지 않으면 응답할 `HTTP 응답 코드`를 의미합니다. 일반적으로 파일이 없음을 나타내는 `404` 로 명시하는 것이 좋습니다. 예시는 아래와 같습니다.

  ```nginx
  location /images/ {
      try_files $uri /images/default.gif;
  }
  ```

  - 위의 예시에서 `URI 경로의 파일` 을 탐색하여 응답에 사용합니다.
  - 만약, `URI 경로의 파일`이 없다면 응답코드 `404`로 응답합니다.

<br>

### [`internal` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#internal)

- 내부 요청이 들어올 경우에만 현재 `location 설정`이 동작할 수 있도록 지정합니다. 
- 외부 요청이 들어올 경우, client 오류응답 404가 반환됩니다.
- `location` 컨텍스트에서 설정할 수 있습니다.

<br>

### [`error_page` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#error_page)

```nginx
error_page code ... [=[response]] uri;
```

- 지정된 오류에 대해 client에게 노출할 URI를 지정합니다.

- `code`는 http 응답코드를 의미합니다. 

- `uri`는 응답코드가 `code`와 같을 때 내부 리다이렉트할 URI를 의미합니다.

- 예시는 아래와 같습니다.

  ```nginx
  error_page 404             /404.html;
  error_page 500 502 503 504 /50x.html;
  ```

- 위와 같이 구성하면, 명시된 URI로 내부 리다이렉트(internal redirect)가 발생합니다. redirect될 때, request method는 `GET`으로 변경되어 호출됩니다.

- `=response` 구문은 `code`로 전달받은 응답코드를 변경합니다. 예시는 아래와 같습니다.

  ```nginx
  error_page 404 =200 /empty.gif;
  # 404 응답코드에 대해 /empty.gif 로 보내고, 응답코드는 200으로 변경됩니다.
  ```

- 오류 응답에 대한 처리가 프록시 서버에서 진행되고, 프록시 서버가 다른 응답 코드(예: 200, 302, 401, 404 등)를 반환할 경우, 해당 코드로 응답할 수 있습니다. 예시는 아래와 같습니다.

  ```nginx
  error_page 404 = /404.php;
  ```

- 내부 리다이렉션 중 **URI와 메서드를 변경할 필요가 없으면** `named location`으로 오류 처리를 전달할 수 있습니다. 예시는 아래와 같습니다.

  ```nginx
  location / {
      error_page 404 = @fallback;
  }
  
  location @fallback {
      proxy_pass http://backend;
  }
  ```

- 만약, uri 처리 중에 오류가 발생했다면 마지막 발생한 오류의 응답코드가 client에 반환됩니다.

- 오류 처리를 위해 `내부 URI 리다이렉트`가 아닌 `URL 리다이렉트`를 사용할 수도 있습니다. 예시는 아래와 같습니다.

  ```nginx
  error_page 403      http://example.com/forbidden.html;
  error_page 404 =301 http://example.com/notfound.html;
  ```

  - `URL 리다이렉트` 의 경우, 응답코드는 302로 client에게 반환됩니다. `=response` 구문을 이용하여 다른 `redirect 응답코드`(301, 302, 303, 307, 308)로 변경할 수는 있습니다.

- 현재 컨텍스트에서 `error_page` 구문이 없을 경우, 상위 레벨의 컨텍스트 설정을 상속하여 사용합니다.

- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [ngx_http_core_module 모듈에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_core_module.html#variables)

#### 시간 관련

- `$time_iso8601` : ISO 8601 표준에 따라 `yyyy-MM-ddTHH:mm:ss±HHMM` 형태의 시간 값을 노출합니다. 예시는 다음과 같습니다. `2024-02-10T15:30:00+09:00` 개인적으로 위의 `$time_local` 형태 보다 익숙한 형태이므로 `time_iso8601`로 사용하는 것을 권합니다.
- `time_local` : Nginx 에서 기본적으로 사용하는 형태의 시간 값을 노출합니다.

#### client 관련

- `$remote_addr` : client IP 주소
- `$binary_remote_addr` : 16진수 형태의 client IP 주소, IPv4 주소의 경우 4바이트 길이, IPv6 주소의 경우 16바이트 길이를 가집니다.
  - 예시는 아래와 같습니다.
  - client IP주소가 255.255.255.255 일 경우, `0xffffffff`가 됩니다.
- `$remote_port` : client 의 포트번호
- `$remote_user` : HTTP Basic 인증을 거친 후, 제공되는 사용자의 이름
- `$http_user_agent` : client 가 접속한 환경 User-Agent

<br>

#### client 측에서 전송한 request 관련

- `$request_id` : 생성된 고유 요청 식별자(16진수 형태)
- `$request_length` : request의 전체 길이 (request line, header, body 모두 포함)
- `$request_time` : `client 에서 전달된 request 의 첫 번째 바이트를 읽은 후, 요청이 처리되기까지` 경과된 시간 (초 단위, 소수점으로 밀리초단위까지 계산하여 노출)
- `$request_method` : request 메소드
- `$request` : 원본 request line
  - `$scheme` : request의 scheme으로 `http` 또는 `https` 값을 가짐.
  - `$host` : 아래의 값 중 존재하는 값을 반환합니다.
    - request line 의 호스트 이름
    - request 의 `Host 헤더 필드 값`
    - request 를 처리할 server name
  - `$request_uri` : 인자를 포함한 원본 request의 URI
    - `$is_args` : 요청 라인에 인수가 있는 경우 `?` 를 반환하고, 그렇지 않은 경우 `""`(빈 문자열)을 반환합니다.
    - `$args` : request line에 존재하는 모든 인자들
      - `$query_string` 와 동일합니다.
      - 예시는 아래와 같습니다.
      - request가 http://localhost?param1=test 일 경우, `$args` 의 값은 `param1=test` 가 됩니다.
    - `$arg_name` : request line에 존재하는 인자 `name`의 값
      - 예시는 아래와 같습니다.
      - request가 http://localhost?param1=test 일 경우, `$arg_param1` 의 값은 `test` 가 됩니다.
- `$http_name` : 요청 헤더 필드 `name` 의 값
  - 헤더 필드의 이름에서 `-` 은 `_`로 변경하고, 모두 소문자로 변경하여 표현해야 합니다.
  - 사용 예시는 아래와 같습니다.
  - 요청 헤더 필드 `X-Forwarded-For`의 값을 표현하려면, `$http_x_forwarded_for` 으로 사용해야 합니다.
- `$content_type` : 요청 헤더 필드 `Content-Type` 의 값
- `$content_length` : 요청 헤더 필드 `Content-Length` 의 값
- `$http_cookie` : Cookie 값
  - `$cookie_name` : Cookie `name` 의 값
- `$request_body` : request body
- `$request_completion` : request가 완료되면 `“OK”`, 그렇지 않으면 `""(빈 문자열)`을 반환 합니다.

<br>

#### server 관련

- `$server_name` : request를 처리하는 server의 이름
- `$server_addr` : request를 처리하는 server의 주소
  - 이 변수의 값을 계산하려면 일반적으로 한 번의 시스템 호출이 필요합니다. 시스템 호출을 방지하려면 `listen 지시어` 에 IP주소를 명시하고, `bind` 파라미터를 사용해야 합니다.
- `$server_port` : request를 처리하는 server의 포트번호
- `$realpath_root` : 현재 request에 대해 `root` 또는 `alias` 지시문의 값에 해당하는 절대 경로
- `$request_filename` : 현재 request에 대해 `root` 또는 `alias` 지시문의 값과 request의 URI를 기반으로 만들어지는 파일의 경로

<br>

#### server 측에서 전송한 response 관련

- `$status` : 응답 코드
- `$sent_http_name` : 응답 헤더 필드 `name` 의 값
  - 헤더 필드의 이름에서 `-` 은 `_`로 변경하고, 모두 소문자로 변경하여 표현해야 합니다.
  - 사용 예시는 아래와 같습니다.
  - 응답 헤더 필드 `Content-Type`의 값을 표현하려면, `sent_http_content_type` 으로 사용해야 합니다.
- `$bytes_sent` : 클라이언트로 전송된 바이트 수
- `$body_bytes_sent` : 응답 헤더를 제외한 클라이언트에 전송된 바이트 수

<br>