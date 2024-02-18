# Nginx 구성

 Nginx 설치되면 아래 경로에 실행파일과 구성파일이 위치합니다.

### Nginx 실행파일 위치

- `/usr/sbin/nginx`

<br>

### Nginx 구성파일 위치

- `/etc/nginx/`
  - `nginx.conf` : 가장 기본 설정파일로 모든 설정파일에 대한 진입점입니다.
  - `conf.d` : 기본 HTTP 설정 파일을 저장하는 디렉토리입니다. 이 디렉토리에서 `~.conf` 파일명을 가지는 모든 설정파일은 `nginx.conf` 파일의 http 블록에 포함됩니다.

<br>

## Nginx 초기 구성파일 - /etc/nginx/nginx.conf

여기서는 `지시어 (directive)`와 `events`, `http`, `server` 컨텍스트에 대해서 설명하지 않습니다. 

자세한 내용은 아래 페이지를 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20Beginner%E2%80%99s%20Guide.md 

```nginx
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

<br>

각각의 설정 지시어(directive)와 값(value)을 살펴보겠습니다.

### [`user` 지시어](https://nginx.org/en/docs/ngx_core_module.html#user)

```nginx
user user [group];
```

- worker process들이 사용할 `user` , `group` 권한을 지정합니다.
- `group` 값은 생략하면, `user` 값과 동일한 이름의 `group`으로 지정됩니다.
- 기본 설정 값은 `user nobody nobody;` 입니다.

<br>

### [`worker processes` 지시어](https://nginx.org/en/docs/ngx_core_module.html#worker_processes)

```nginx
worker_processes number | auto;
```

* nginx에는 master process와 worker process가 있는데, worker processes는 말 그대로 request를 처리하는 프로세스들을 의미합니다.
* 성능 튜닝을 위한 확실한 숫자를 모를 경우, worker processes의 개수는 CPU의 코어 개수와 맞춰주는 것이 좋습니다.
* `auto` 로 설정하면, worker process를 CPU 코어의 개수로 자동으로 설정합니다.
* 기본 설정 값은 `worker_processes 1;` 입니다.

<br>

### [`error_log` 지시어](https://nginx.org/en/docs/ngx_core_module.html#error_log)

```nginx
error_log file [level]
```

- Nginx 서버에서 발생하는 오류와 관련된 정보를 저장하게 하는 설정입니다.
- `file`은 오류 로그를 저장할 파일 경로를 지정합니다.
- `level`은 기록할 오류 로그의 레벨을 지정합니다. 
  - `level`로 지정될 수 있는 값들은 심각도가 높아지는 순으로 나열하면 다음과 같습니다. `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, or `emerg`입니다. 
  - `info` 레벨로 설정하면 `info` 레벨보다 심각도가 높은 로그들이 모두 기록됩니다.
  - 주의: `debug`레벨이 정상적으로 동작하려면 Nginx를 빌드할 때, `--with-debug` 옵션을 함께 주어야 합니다.
  - 기본 값은 `error`입니다.
- `main 컨텍스트`, `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트`에서 설정할 수 있으며, 하위 컨텍스트에 정의된 설정이 적용됩니다. 
- `main 컨텍스트` 까지 설정이 없으면, 기본 설정 값인 `error_log logs/error.log error;` 가 사용됩니다.
- 각 `server` 별로 다른 `file`에 오류 로그를 저장하도록 설정하는 것이 좋습니다.

<br>

### [`pid` 지시어](https://nginx.org/en/docs/ngx_core_module.html#pid)

```nginx
pid file
```

- Nginx `main 프로세스`의 ID를 저장할 파일을 명시합니다.
- `main 프로세스`를 `kill` 명령어를 이용하여 종료하고 싶을 때, 참고하기 위함입니다.
- 기본 값은 `pid logs/nginx.pid` 입니다.
- `main 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`worker_connections` 지시어](https://nginx.org/en/docs/ngx_core_module.html#worker_connections)

```nginx
events {
    worker_connections  number;
}
```

* 하나의 worker process는 여러 개의 request와 connection을 맺을 수 있습니다.
* `number`는 하나의 worker process가 맺을 수 있는 connection의 최대 값을 지정합니다.
* `Nginx 서버가 처리할 수 있는 동시접속 수` = `worker_processes` X `worker_connections` 이므로 서버에 필요한 동시접속 트래픽을 고려하여 설정하도록 합니다. (테스팅 등 트래픽을 예측하는 작업이 필요합니다.)
* `worker_connections` 지시어는 `events 컨텍스트` 안에서 설정할 수 있습니다.
* 기본 설정 값은 `worker_connections  512;` 입니다.

<br>

### [`use` 지시어](https://nginx.org/en/docs/ngx_core_module.html#use)

```nginx
events {
    use method;
}
```

- request의 connection을 처리할 때, 사용하는 `method` 를 명시합니다.
- Nginx는 connection을 처리하는 방법으로 많은 `method`를 지원합니다. 다만, 설치되는 플랫폼(OS)에 따라 각각의 `method` 사용 가능 여부는 다릅니다. 그래서, Nginx에서는 사용 가능한 가장 효율적인 method를 자동으로 선택하여 사용합니다.
- `method` 의 값으로는 아래와 같은 값을 가질 수 있습니다. [참고](https://nginx.org/en/docs/events.html)
  - `select`: 표준 `method` 입니다. 다른 효율적인 `method`를 사용할 수 없는 플랫폼일 때, 이 `method`를 지원하는 모듈이 자동으로 빌드됩니다.
  - `poll`: 표준 `method` 입니다. 다른 효율적인 `method`를 사용할 수 없는 플랫폼일 때, 이 `method`를 지원하는 모듈이 자동으로 빌드됩니다.
  - `kqueue`: FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0, and macOS 플랫폼에서 사용 가능한 효율적인 `method` 입니다.
  - `epoll`: Linux 2.6+ 에서 사용 가능한 효율적인 `method` 입니다.
  - `/dev/poll`: Solaris 7 11/99+, HP/UX 11.22+ (eventport), IRIX 6.5.15+, and Tru64 UNIX 5.1A+ 에서 사용 가능한 효율적인 `method` 입니다.
  - `eventport`:  Solaris 10+ 에서 사용 가능한 효율적인 `method` 입니다.

- **Nginx가 기본적으로 가장 효율적인 `method` 를 사용할 것이기 때문에 일반적으로는 해당 설정을 명시할 필요는 없습니다.**
- `use` 지시어는 `events 컨텍스트` 안에서 설정할 수 있습니다.

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

### [`include` 지시어](https://nginx.org/en/docs/ngx_core_module.html#include)

```nginx
include file
```

- 지정된 `file`의 내용을 그대로 가져옵니다.
- `file`은 하나의 파일 또는 `*`를 이용한 여러 개의 파일을 지정할 수 있습니다.
- 지정된 파일의 내용들은 올바른 문법의 directive와 block들로 구성되어야 합니다.
- `모든 컨텍스트` 에서 설정할 수 있습니다.

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

### [`charset` 지시어](https://nginx.org/en/docs/http/ngx_http_charset_module.html#charset)

```nginx
 charset charset | off;
```

* 응답헤더의 `Content-Type` 필드에 지정한 `charset` 값을 추가합니다.
* `off` 로 값을 설정하면 응답헤더의 `Content-Type` 필드를 추가하지 않습니다.
* 일반적으로 `charset utf-8;`로 설정합니다.
* 기본 설정 값은 ` charset off;` 입니다.
* `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`log_format` 지시어](https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format)

```nginx
log_format name [escape=default|json|none] string ...;
```

* 로그의 형식를 지정합니다.

* `escape` 는 특수문자 escape 처리를 하는 방식을 지정합니다.

  * `default` 로 설정하면 `"` 문자, `\` 문자, `charater value가 32보다 작거나 126보다 큰 문자`에 대해서 `\xXX` 형태로 escape 처리됩니다. 값이 없는 경우 `-`로 표시합니다.
  * `json`으로 설정하면 JSON 문자열에서 허용되지 않는 모든 문자들을 escape 처리합니다. 로그를 JSON 형태로 직렬화하여 저장하고, 추후에 역직렬화하여 확인하고 싶을 때 사용하는 방식입니다.
  * `none`으로 설정하면 escape 처리를 하지 않습니다.
  * `escape 파라미터`의 기본 설정 값은 `escape=default` 입니다.

* `string ...` 은 로깅할 변수들과 변수들이 조합되는 형식을 지정합니다.

  * 로그 형식에는 공통 변수와 로그 작성 시에만 존재하는 변수들이 포함될 수 있습니다.

  * `$bytes_sent` : 클라이언트로 전송되는 바이트의 크기를 의미합니다.

  * `$connection` : 연결 일련번호를 의미합니다.

  * `$connection_requests` : `하나의 connection` 을 통해 만들어진 `request`의 현재 번호를 의미합니다. 추측컨대, keep-alive를 통해 유지된 connection을 통해 만들어진 request 중 현재가 몇 번째 request인지를 의미하는 것으로 생각됩니다.

  * `$msec` : 로그 쓰기 시점의 시간을 의미합니다. `milliseconds 단위`

  * `$pipe` : request가 server로 전송되면 `p`를 기록하고, 그렇지 않으면 `.`을 기록합니다.

  * `$request_length` : request 의 길이를 의미합니다. (request 라인, request 헤더 및 request body 포함)

  * `$request_time` : 요청을 처리하는데 걸리는 시간을 초 단위로 나타냅니다. (다만, milliseconds 단위까지 보여줍니다. `ex: 1.023`) client로 부터 첫 바이트를 읽어들인 시점과 client에게 마지막 응답 바이트를 보낸 후 로그를 기록하는 시점 사이의 소요시간을 나타냅니다.

  * `$status` : http 응답 코드를 의미합니다.

  * `$time_local` : `dd/MMM/yyyy:HH:mm:ss ±HHMM` 형태의 시간 값을 노출합니다. 예시는 다음과 같습니다. `10/Feb/2024:15:30:00 +0900`

  * `$time_iso8601` : ISO 8601 표준에 따라 `yyyy-MM-ddTHH:mm:ss±HHMM` 형태의 시간 값을 노출합니다. 예시는 다음과 같습니다. `2024-02-10T15:30:00+09:00` 개인적으로 위의 `$time_local` 형태 보다 익숙한 형태이므로 `time_iso8601`로 사용하는 것을 권합니다.

  * nginx 기본 구성 파일에는 아래와 같은 형태로 설정되어 있습니다.

    ```nginx
    log_format combined '$remote_addr - $remote_user [$time_local] '
                        '"$request" $status $body_bytes_sent '
                        '"$http_referer" "$http_user_agent"';
    ```

  * 개인적으로 설정한다면 아래와 같은 형태로 할 것 같습니다.

    ```nginx
    log_format improved '[$time_iso8601] $remote_addr $http_x_forwarded_for'
                        '$request $status $request_time '
                        '$http_referer $http_user_agent';
    
    #첫번째줄: 시간, 요청IP주소, 요청을 전달한 프록시서버 IP주소 목록
    #두번째줄: 요청URL, 응답코드, 응답소요시간
    #세번째줄: 요청을 수행한 referer페이지 URL, 요청한 장비 agent 정보
    ```

  * 변수로 사용될 수 있는 것들은 [참고자료](https://nginx.org/en/docs/http/ngx_http_core_module.html#variables)를 확인해주세요.

* `http 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`access_log` 지시어](https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log)

```nginx
access_log path [format [buffer=size] [gzip[=level]] [flush=time] [if=condition]];
또는
access_log off;
```

* Nginx에 access 로그를 저장할 파일의 경로와 로그 형식을 지정합니다. 

* `path` 는 access 로그를 저장할 파일 경로를 의미합니다. **해당 파일은 `user` 지시어에서 명시한 사용자의 권한으로 접근이 가능해야 합니다.**

* `format` 은 access 로그를 저장할 때, 사용할 로그 형식을 의미합니다. `log_format` 지시어에서 정의한 값을 사용합니다.

* `buffer` , `flush` 는 로그 버퍼링을 위해 사용하는 기능입니다. 자세한 설명은 docs를 참고하세요.

* `gzip` 은 버퍼링된 데이터는 파일에 쓰기 전에 압축할 때, 사용하는 기능입니다. 자세한 설명은 docs를 참고하세요.

* 아래와 같은 형태로 설정할 수 있습니다.

* ```nginx
  access_log  /path/to/logs/nginx/nginx-1.18.0/instance_name/access.log  improved;
  ```

* `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

* 기본 설정 값은 `access_log logs/access.log combined;` 입니다. `combined`는 기본 Nginx 설정 파일에서 정의된 `log_format` 지시어의 값입니다.

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

### [`gzip` 관련 지시어](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

```nginx
gzip on | off;gzip on;
gzip_disable regex ...;
gzip_comp_level level;
gzip_types text.html text/css application/javascript application/json font/wotf font/wotf2 image/x-icon;
gzip_buffers number size;
gzip_http_version 1.0 | 1.1;
gzip_min_length length;
gzip_proxied off | expired | no-cache | no-store | private | no_last_modified | no_etag | auth | any ...;
gzip_vary on | off;
```

* gzip 압축을 이용하면, 전송할 파일의 크기가 줄어 네트워크 전송속도를 향상시킬 수 있습니다.
* `gzip on | off`
    * gzip압축을 사용할지 여부를 설정합니다.
    * 기본 설정 값은 `gzip off;` 입니다.
* `gzip_disable regex`
    * `regex` 에서 지정한 정규표현식이 `request의 User-Agent헤더 필드 값`과 매칭되면 gzip 압축을 비활성화합니다.
    * Microsoft IE 4 \~ 6 버전에서는 압축을 지원하지 않기 때문에 gzip 압축대상에서 예외처리합니다.
    * `"msie6"` 는 특별히 제공하는 마스킹으로 정규표현식으로 `"MSIE[4-6]\."` 와 일치합니다.
* `gzip_comp_level`
    * 압축레벨을 1 ~ 9 중 하나로 설정할 수 있습니다.
    * 압축되는 크기와 CPU 자원사용량을 고려했을 때, 5레벨이 적절합니다.
    * 압축률 참조 - [https://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level](https://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level)
    * 기본 설정 값은 `gzip_comp_level 1;` 입니다.
* `gzip_types`
    * 지정한 MIME 타입을 가지는 응답 파일들에 대해서만 압축을 진행합니다.
    * 기본적으로 `text/html` MIME 타입은 항상 압축이 진행됩니다.
    * 기본 설정 값은 `gzip_types text/html;` 입니다.
* `gzip_buffers`
    * 버퍼를 두어서, 압축의 속도를 빠르게 할 수 있습니다.
    * 파일 전송 시, 버퍼를 사용하여 전송속도를 빠르게하는 원리와 비슷합니다.
    * `gzip_buffers 16 8k`로 설정하면 8KB짜리 버퍼를 16개 두게 됩니다.
    * 기본 설정 값은 `gzip_buffers 32 4k` 또는 `gzip_buffers 16 8k;` 입니다. 
    * 기본 설정 값의 버퍼 크기는 메모리 페이지 1개의 크기와 같습니다.
* `gzip_http_version`
    * 응답을 압축하는 데 필요한 `request의 최소 HTTP 버전`을 설정합니다.
    * `1.0` 또는 `1.1`로 설정할 수 있습니다.
    * 기본 설정 값은 `gzip_http_version 1.1;` 입니다.
* `gzip_min_length`
    * gzip 압축을 진행할 `응답의 최소 크기` 를 지정하는 옵션입니다.
    * `length` 는 gzip 압축을 진행할 `응답의 최소 크기 (바이트)` 를 의미합니다.
    * `Content-Length 응답 헤더필드 값`과 `length`를 비교하여 더 클 경우에만 gzip 압축합니다.
    * 기본 설정 값은 `gzip_min_length 20;` 입니다.
    * **너무 작은 파일을 압축할 경우 발생하는 비효율을 없애기 위한 설정입니다.**
* `gzip_proxied`
    * Proxy를 통해서 들어오는 요청에 대해서도 gzip 압축 후, 응답할 것인지에 대한 설정입니다.
    * Proxy를 통해서 들어오는 요청을 구분하는 기준은 `요청 헤더필드 Via` 가 있는지 없는지로 구분합니다.
    * 가질 수 있는 값은 [참고자료](https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied)를 확인합니다.
    * 기본 값은 `gzip_proxied off;` 입니다.
* `gzip_vary`
    * `응답헤더에 Vary: Accept-Encoding 필드 값`을 추가할지 말지를 설정합니다.
    * client 측에서 압축이 됐는지를 확인할 수 있게 합니다.
    * 기본 설정 값은 `gzip_vary off;` 입니다.

- 예시 설정은 아래와 같습니다.

  ```nginx
  gzip on;
  gzip_disable "msie6";
  gzip_comp_level 5;
  gzip_types text.html text/css application/javascript application/json font/wotf font/wotf2 image/x-icon;
  gzip_buffers 16 8k;
  gzip_min_length 1024;
  gzip_proxied any;
  gzip_vary on;
  ```

<br>

## Nginx 초기 구성파일 - /etc/nginx/conf.d/default.conf

```nginx
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

<br>

### [`listen` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#listen)

listen 지시어는 request를 처리할 server 블록을 선택할 때, 사용되는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20How%20nginx%20processes%20a%20request.md

<br>

### [`server_name` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#server_name)

server_name 지시어는 request를 처리할 server 블록을 선택할 때, 사용되는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20Server%20names.md

<br>

### [`location` 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)

location 지시어는 request를 처리할 location 블록을 선택할 때, 사용되는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20location%20%EC%A7%80%EC%8B%9C%EC%96%B4.md

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

### 접근 제한 기능을 제공하는 모듈

- `접근 제한 기능`을 제공하는 모듈은 아래와 같이 존재합니다.
  - client IP 기반의 접근 제한: https://nginx.org/en/docs/http/ngx_http_access_module.html
  - password 기반의 접근 제한: https://nginx.org/en/docs/http/ngx_http_auth_basic_module.html
  - sub request 기반의 접근 제한: https://nginx.org/en/docs/http/ngx_http_auth_request_module.html
  - JWT 기반의 접근 제한: https://nginx.org/en/docs/http/ngx_http_auth_jwt_module.html

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

### [`index` 지시어](https://nginx.org/en/docs/http/ngx_http_index_module.html#index)

`index 지시어`는 `request의 URI`가 `/`로 끝났을 때, `root` 지시자 또는 `alias` 지시자와 `request 의 URI`가 결합된 파일 경로에 존재하는 기본 응답파일을 명시합니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20Server%20names.md

<br>

### [`internal` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#internal)

- 내부 요청이 들어올 경우에만 현재 `location 설정`이 동작할 수 있도록 지정합니다. 
- 외부 요청이 들어올 경우, client 오류응답 404가 반환됩니다.
- `location` 컨텍스트에서 설정할 수 있습니다.

<br>

### [`set` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#set)

```nginx
set $variable value;
```

- 지정된 변수의 값을 설정합니다. 
- 변수의 값에는 텍스트, 변수 등이 포함될 수 있습니다.
- `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`if` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#if)

```nginx
if (condition) { ... }
```

- `condition` 을 참/거짓을 판단하고 참이면, 명시한 블록이 실행됩니다.

- `condition` 은 아래의 방식으로 참/거짓을 판단합니다.

  - `condition` 위치에 `$variable_name` 가 존재할 경우, `variable_name`의 값이 `"0"` 또는 `""`라면  거짓으로 판단합니다.
  - `condition` 위치에 `$variable_name` 과 `=` 연산자 또는 `!=` 연산자를 함께 사용하여 비교구문을 사용할 수 있습니다.
  - `condition` 위치에 `$variable_name` 과 `~` 연산자 또는 `~*` 연산자를 함께 사용하여 정규표현식 매칭구문을 사용할 수 있습니다. `~` 연산자는 대/소문자를 구분하는 정규표현식 매칭입니다. `~*` 연산자는 대/소문자를 구분하지 않는 정규표현식 매칭입니다.
  - `condition` 위치에 `$variable_name` 과 `!~` 연산자 또는 `!~*` 연산자를 함께 사용하여 정규표현식 매칭구문을 사용할 수 있습니다. `!~` 연산자는 대/소문자를 구분하면서 정규표현식 매칭되지 않을 때 블록을 수행합니다. `!~*` 연산자는 대/소문자를 구분하지 않는 정규표현식 매칭되지 않을 때 블록을 수행합니다.
  - 정규표현식이 `}` 문자 또는 `;` 문자를 포함한다면 정규표현식 전체를 `'` 문자 또는 `"` 문자로 감싸야합니다.
  - `condition` 위치에 `-f` 연산자 또는 `!-f` 연산자를 함께 사용하여 파일의 존재유무를 조건으로 사용할 수 있습니다.
  - `condition` 위치에 `-d` 연산자 또는 `!-d` 연산자를 함께 사용하여 디렉토리의 존재유무를 조건으로 사용할 수 있습니다.
  - `condition` 위치에 `-e` 연산자 또는 `!-e` 연산자를 함께 사용하여 파일, 디렉토리, 심볼릭 링크의 존재유무를 조건으로 사용할 수 있습니다.
  - `condition` 위치에 `-x` 연산자 또는 `!-x` 연산자를 함께 사용하여 실행가능한 파일의 존재유무를 조건으로 사용할 수 있습니다.

- 예시는 아래와 같습니다.

  ```nginx
  if ($http_user_agent ~ MSIE) {
      rewrite ^(.*)$ /msie/$1 break;
  }
  
  if ($http_cookie ~* "id=([^;]+)(?:;|$)") {
      set $id $1;
  }
  
  if ($request_method = POST) {
      return 405;
  }
  
  if ($slow) {
      limit_rate 10k;
  }
  
  if ($invalid_referer) {
      return 403;
  }
  ```

- `server 컨텍스트`, `location 컨텍스트` 에서 설정할 수 있습니다.

<br>

### [`break` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#break)

```nginx
break;
```

- 현재 수행하고 있는 `ngx_http_rewrite_module 모듈의 지시어` 처리를 중단합니다.
- **하지만, `location 블록` 안에서 `break;` 를 사용해도 `request의 처리`는 계속됩니다. 즉, `break;` 구문 이후에 남아있는 `location 블록 내 지시어`를 모두 수행하고 종료됩니다.**
- `server 컨텍스트`, `location 컨텍스트`, `if 컨텍스트`에서 설정할 수 있습니다.

<br>

### [`return` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return)

```nginx
return code [text];
return code URL;
return URL;
```

- `request의 처리`를 중지하고 지정된 응답코드를 client에 반환합니다.
- `code` 는 client에 반환되는 응답코드를 의미합니다.
- `text` 는 client에 반환되는 응답 body 문구를 의미합니다.
- client에 응답을 직접하는 것이 아니고 리다이렉트 기능을 사용하려면 `code`와 `URL`을 함께 명시합니다.
  - 리다이렉트 시, 사용가능한 `code` 값은 301, 302, 303, 307, 308 입니다.
  - `code` 값을 생략할 경우 기본 응답코드는 302가 됩니다.
  - `URL` 은 리다이렉트할 URL을 의미합니다.
  - `URL` 값은  `http://` 또는 `https://` 또는 `$scheme` 로 시작해야됩니다.
  - 리디렉션 `URL` 을 이 `서버의 로컬 URI`로 지정할 수 있습니다. 이 때, 실제 리디렉션 URL은 `$scheme` , `server_name_in_redirect`, `port_in_redirect` 지시문에 따라 구성됩니다.
- `code`를 비표준코드인 `444`로 지정하면 응답 헤더를 전송하지 않고 연결을 종료합니다.

- `server 컨텍스트`, `location 컨텍스트`, `if 컨텍스트`에서 설정할 수 있습니다.

<br>

### [`rewrite` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite)

```nginx
rewrite regex replacement [flag];
```

- 지정된 `정규표현식`이 `요청 URI`와 매칭되면 `요청 URI`가 `대체 문자열`로 변경됩니다.

- `regex` 는 요청 URI와 매칭을 수행할 `정규표현식`을 의미합니다.

- `replacement` 는 `정규표현식`이 `요청 URI`와 매칭되면 `요청 URI`를 대체할 `대체 문자열`을 의미합니다.

- `flag` 지시어는 아래의 값 중 하나를 가질 수 있습니다.

  - `last` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`,  다음 처리는 중단하고 변경된 URI와 매칭되는 새 `location` 검색을 시작합니다.
  - `break` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`,  다음 처리는 중단합니다.
  - `redirect` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`,  다음 처리는 중단하고 변경된 URI와 함께 `일시적인 redirect를 뜻하는 응답코드 302`를 반환합니다.
  - `permanent` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`,  다음 처리는 중단하고 변경된 URI와 함께 `영구적인 redirect를 뜻하는 응답코드 301`를 반환합니다.
  - `redirect 지시어` 와 `permanent 지시어` 로 반환하는 URI는 `$scheme` , `server_name_in_redirect`, `port_in_redirect` 지시어에 의해 실제 리디렉션 URL로 변경되어 응답됩니다.

- `rewrite 지시어` 의 동작은 아래와 같습니다.

  - `rewrite 지시어` 는 구성 파일에 나타나는 순서대로 순차적으로 실행됩니다.
  - `request URI` 와 `rewrite의 정규표현식` 이 매칭될 경우, `flag` 에 의해 적절하게 처리됩니다.
  - `rewrite 지시어` 에 의해 변경된 `replacement` 가 `http://`, `https://` , `$scheme` 이면 처리가 중단되고 client로 변경된 URL (`redirect 처리`)을 반환합니다.

- 정규표현식에 의해 `캡처된 request URI`에 `request argument`를 포함하여 `replacement` 되는 구문에도 `request argument` 를 포함될 수 있습니다. 이러한 동작을 원치 않는다면, `request argument`가 추가되지 않도록 `replacement 지시어`의 끝에 `?`를 추가합니다.

  - 예시는 아래와 같습니다.

    ```nginx
    rewrite ^/users/(.*)$ /show?user=$1? last;
    ```

- rewrite 설정 예시는 다음과 같습니다.

  ```nginx
  location /download/ {
      rewrite ^(/download/.*)/media/(.*)\..*$ $1/mp3/$2.mp3 break;
      rewrite ^(/download/.*)/audio/(.*)\..*$ $1/mp3/$2.ra  break;
      return  403;
  }
  ```

- `server 컨텍스트`, `location 컨텍스트`, `if 컨텍스트`에서 설정할 수 있습니다.

<br>

### [`rewrite_log` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log)

```nginx
rewrite_log on | off;
```

- `ngx_http_rewrite_module 모듈 지시어`의 처리 결과를 `error_log`에 `notice 레벨`로 로깅하는 기능을 활성화 또는 비활성화합니다.

- 기본 설정 값은 `rewrite_log off;` 입니다.
- `http 컨텍스트`, `server 컨텍스트`, `location 컨텍스트`, `if 컨텍스트`에서 설정할 수 있습니다.

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

###  그 외 불필요한 설정

```nginx
server {
    # 생략
    
    
	# proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

* WebAppServer(WAS)로 SpringBoot 서버를 이용하기 때문에 위의 PHP 관련 설정은 필요 없습니다. 이미 주석처리가 되어있지만, 혼동을 피하기 위해 제거합니다.
* `htaccess file` 접근 관련 설정도 Apache 관련 설정이므로 제거합니다.

<br>

### [ngx_http_core_module 모듈에서 지원하는 변수](https://nginx.org/en/docs/http/ngx_http_core_module.html#variables)

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

<br>

#### server 관련

- `$server_name` : request를 처리하는 server의 이름
- `$server_addr` : request를 처리하는 server의 주소
  - 이 변수의 값을 계산하려면 일반적으로 한 번의 시스템 호출이 필요합니다. 시스템 호출을 방지하려면 `listen 지시어` 에 IP주소를 명시하고, `bind` 파라미터를 사용해야 합니다.
- `$server_port` : request를 처리하는 server의 포트번호

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

## Nginx Reverse Proxy 구성 - [ngx_http_proxy_module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)

### Reverse Proxy를 사용하여 얻는 효과

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

## Nginx Reverse Proxy 서버 그룹 구성 - [ngx_http_upstream_module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)

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

