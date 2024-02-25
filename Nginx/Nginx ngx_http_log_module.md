# Nginx ngx_http_log_module

https://nginx.org/en/docs/http/ngx_http_log_module.html

ngx_http_log_module 모듈은 지정된 형식으로 요청 로그를 작성합니다.

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

  * 변수로 사용될 수 있는 것들은 아래와 같습니다.

    - [http core 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_core_module-%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
    - [http proxy 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_proxy_module-%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
    - [http upstream 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_upstream_module%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)

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