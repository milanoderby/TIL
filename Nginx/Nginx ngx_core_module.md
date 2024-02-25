# Nginx ngx_core_module

https://nginx.org/en/docs/ngx_core_module.html

nginx 의 main, events 컨텍스트에서 정의되는 지시어와 관련하여 설명합니다.

<br>

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