# Nginx 구성 예시 - API 서버

API 서버를 위한 Nginx 구성 파일을 작성합니다.

## /etc/nginx/nginx.conf 파일

[Nginx 최초 구성 파일](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#nginx-%EC%B4%88%EA%B8%B0-%EA%B5%AC%EC%84%B1%ED%8C%8C%EC%9D%BC---etcnginxnginxconf) 과 **다른 내용에 대해서만 설명**을 하겠습니다.

```nginx
user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/nginx-error.log notice;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    server_tokens off;
    charset utf-8;

    log_format custom_json escape=json '{ '
        '"time": "$time_iso8601", '
        '"client": { '
            '"remote_addr": "$remote_addr", '
            '"remote_port": "$remote_port", '
            '"http_user_agent": "$http_user_agent" '
        '}, '
        '"request": { '
            '"request_time": "$request_time", '
            '"request_method": "$request_method", '
            '"scheme": "$scheme", '
            '"host": "$host", '
            '"http_host": "$http_host", '
            '"proxy_host": "$proxy_host", '
            '"request_uri": "$request_uri", '
            '"content_type": "$content_type", '
            '"http_x_forwarded_for": "$http_x_forwarded_for", '
            '"proxy_add_x_forwarded_for": "$proxy_add_x_forwarded_for", '
            '"http_cookie": "$http_cookie", '
            '"request_body": "$request_body", '
            '"request_completion": "$request_completion" '
        '}, '
        '"server": { '
            '"server_name": "$server_name", '
            '"server_addr": "$server_addr", '
            '"server_port": "$server_port", '
            '"upstream_addr": "$upstream_addr", '
            '"realpath_root": "$realpath_root", '
            '"request_filename": "$request_filename" '
        '}, '
        '"response": { '
            '"status": "$status", '
            '"upstream_status": "$upstream_status", '
            '"upstream_response_time": "$upstream_response_time" '
        '} '
    '}';

    access_log  /var/log/nginx/nginx-access.log  custom_json;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout 0;

    gzip on;
    gzip_disable "msie6";
    gzip_comp_level 5;
    gzip_types application/json;
    gzip_min_length 1024;
    gzip_proxied any;
    gzip_vary on;
    
    upstream backend {
        server localhost:8080;
    }

    include /etc/nginx/conf.d/*.conf;
}
```

### [`server_tokens` 지시어](https://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens)

``` nginx
server_tokens off;
```

- Nginx의 기본 오류페이지와 응답 헤더의 `Server`값에서 nginx 버전을 보여줄지 말지를 설정합니다.
- 보안을 위해 Nginx 버전은 사용자에게 노출되지 않는 것이 좋으므로 `server_tokens off;`로 설정합니다.

<br>

### [`charset` 지시어](https://nginx.org/en/docs/http/ngx_http_charset_module.html#charset)

```nginx
 charset utf-8;
```

- 응답헤더의 `Content-Type` 필드에 지정한 `charset` 값을 추가합니다.
- 일반적으로 사용되는 캐릭터 셋인 `UTF-8`로 설정합니다.
- `charset utf-8;`로 설정합니다.

<br>

### [`log_format` 지시어](https://nginx.org/en/docs/http/ngx_http_log_module.html#log_format)

```nginx
log_format custom_json escape=json '{ '
    '"time": "$time_iso8601", '
    '"client": { '
        '"remote_addr": "$remote_addr", '
        '"remote_port": "$remote_port", '
        '"http_user_agent": "$http_user_agent" '
    '}, '
    '"request": { '
        '"request_time": "$request_time", '
        '"request_method": "$request_method", '
        '"scheme": "$scheme", '
        '"host": "$host", '
		'"http_host": "$http_host", '
		'"proxy_host": "$proxy_host", '
        '"request_uri": "$request_uri", '
        '"content_type": "$content_type", '
        '"http_x_forwarded_for": "$http_x_forwarded_for", '
        '"proxy_add_x_forwarded_for": "$proxy_add_x_forwarded_for", '
        '"http_cookie": "$http_cookie", '
        '"request_body": "$request_body", '
        '"request_completion": "$request_completion" '
    '}, '
    '"server": { '
        '"server_name": "$server_name", '
        '"server_addr": "$server_addr", '
        '"server_port": "$server_port", '
        '"upstream_addr": "$upstream_addr", '
        '"realpath_root": "$realpath_root", '
        '"request_filename": "$request_filename" '
    '}, '
    '"response": { '
        '"status": "$status", '
        '"upstream_status": "$upstream_status", '
        '"upstream_response_time": "$upstream_response_time" '
    '} '
'}';

```

- 로그의 형식를 지정합니다.

- `escape` 는 특수문자 escape 처리를 하는 방식을 지정합니다.

  - `json`으로 설정하면 JSON 문자열에서 허용되지 않는 모든 문자들을 escape 처리합니다. 로그를 JSON 형태로 직렬화하여 저장하고, 추후에 역직렬화하여 확인하고 싶을 때 사용하는 방식입니다.

- `escape` 뒤에는 로깅할 변수들과 변수들이 조합되는 형식을 지정합니다.

  - 로그 형식에는 공통 변수와 로그 작성 시에만 존재하는 변수들이 포함될 수 있습니다.

- 아래와 같은 형태의 JSON 데이터를 로그로 전송하기 위해 위와 같이 지정하였습니다.

  ```json
  {
      "time": "time_iso8601",
      "client": {
          "remote_addr": "$remote_addr",
          "remote_port": "$remote_port",
          "http_user_agent": "$http_user_agent"
      },
  	"request": {
  		"request_time": "$request_time",
  		"request_method": "$request_method",
          "scheme": "$scheme",
          "host": "$host",
          "http_host": "http_host",
          "proxy_host": "proxy_host",
          "request_uri": "$request_uri",
          "content_type": "$content_type",
          "http_x_forwarded_for": "$http_x_forwarded_for",
          "proxy_add_x_forwarded_for": "$proxy_add_x_forwarded_for",
          "http_cookie": "$http_cookie",
          "request_body": "$request_body",
          "request_completion": "$request_completion"
  	},
      "server": {
  	    "server_name": "$server_name",
          "server_addr": "$server_addr",
          "server_port": "$server_port",
          "upstream_addr": "$upstream_addr",
          "realpath_root": "$realpath_root",
          "request_filename": "$request_filename",
      },
      "response": {
          "status": "$status",
          "upstream_status": "$upstream_status",
          "upstream_response_time": "$upstream_response_time"
      }
  }
  ```

- 위에서 사용되는 변수는 각각 아래 항목에서 확인 가능합니다.
  - [http core 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_core_module-%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
  - [http proxy 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_proxy_module-%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
  - [http upstream 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#ngx_http_upstream_module%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)

<br>

### [`access_log` 지시어](https://nginx.org/en/docs/http/ngx_http_log_module.html#access_log)

```nginx
access_log  /var/log/nginx/nginx-access.log  custom_json;
```

- Nginx에 access 로그를 저장할 파일의 경로와 로그 형식을 지정합니다.
- `path` 는 access 로그를 저장할 파일 경로를 의미합니다. **해당 파일은 `user` 지시어에서 명시한 사용자의 권한으로 접근이 가능해야 합니다.**
- **또한, 로그를 저장할 파일은 이미 생성되어 있어야 합니다.**
- `format` 은 access 로그를 저장할 때, 사용할 로그 형식을 의미합니다. `log_format` 지시어에서 새로 정의한 `custom_json`값을 사용합니다.

<br>

### [`keepalive` 관련 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html)

``` nginx
keepalive_timeout 0;
```

- keep-alive 란?
  - HTTP Connection은 `connectionless`(비연결성) 특성을 가지고 있습니다. 클라이언트 요청 시마다 매번 connection을 맺고, 응답을 완료하면, 연결을 끊어버립니다.
  - 위와 같은 특성으로 client가 매번 새로운 `연결/해제` 과정에서 오버헤드가 발생합니다.
  - 이러한 오버헤드를 줄이기 위해 `연결을 재사용하도록 유지하는 방식`이 keep-alive입니다.
  - keep-alive를 통해 client와 server의 connection을 유지한다는 것은 서버의 메모리 자원 또한 해제되지 않고, 유지됨을 의미합니다.
  - 그래서, `재사용횟수 제한`, `유지시간 제한`, `서버자원 제한`을 두어 keep-alive connection들을 주기적으로 해제합니다.
- API 서버는 브라우저와 통신하는 서버와는 달리 매우 짧은 시간 동안 많은 요청을 처리하므로 keep-alive 연결이 필요하지 않습니다. 따라서, keep-alive 설정을 최소화하여 서버 리소스를 효율적으로 관리하도록 합니다.

**위 설정은 keep-alive 연결을 즉시 종료하는 것입니다. 이렇게 설정하면 서버가 매 요청마다 새로운 연결을 생성하므로 keep-alive 연결 유지를 위해 사용되는 서버 리소스(메모리)가 없게 됩니다.**

**브라우저와 통신하는 서버가 아니기 때문에 keep-alive 연결을 유지하지 않도록 구성하였습니다.**

<br>

### [`gzip` 관련 지시어](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

```nginx
gzip on;
gzip_disable "msie6";
gzip_comp_level 5;
gzip_types application/json;
gzip_min_length 1024;
gzip_proxied any;
gzip_vary on;
```

- API 서버에서 JSON 데이터를 응답할 때 Nginx의 gzip 압축 설정을 사용하면 대역폭을 절약하고 응답 시간을 줄일 수 있습니다.  
- `gzip on | off`
  - gzip압축을 사용할지 여부를 설정합니다.
  - 기본 설정 값은 `gzip off;` 입니다.
- `gzip_disable regex`
  - `regex` 에서 지정한 정규표현식이 `request의 User-Agent헤더 필드 값`과 매칭되면 gzip 압축을 비활성화합니다.
  - Microsoft IE 4 ~ 6 버전에서는 압축을 지원하지 않기 때문에 gzip 압축대상에서 예외처리합니다.
  - `"msie6"` 는 특별히 제공하는 마스킹으로 정규표현식으로 `"MSIE[4-6]\."` 와 일치합니다.
- `gzip_comp_level`
  - 압축레벨을 1 ~ 9 중 하나로 설정할 수 있습니다.
  - 압축되는 크기와 CPU 자원사용량을 고려했을 때, 5레벨이 적절합니다.
  - 압축률 참조 - https://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level
  - 기본 설정 값은 `gzip_comp_level 1;` 입니다.
- `gzip_types`
  - 지정한 MIME 타입을 가지는 응답 파일들에 대해서만 압축을 진행합니다.
  - 기본적으로 `text/html` MIME 타입은 항상 압축이 진행됩니다.
  - 기본 설정 값은 `gzip_types text/html;` 입니다.
- `gzip_buffers`
  - 버퍼를 두어서, 압축의 속도를 빠르게 할 수 있습니다.
  - 파일 전송 시, 버퍼를 사용하여 전송속도를 빠르게하는 원리와 비슷합니다.
  - `gzip_buffers 16 8k`로 설정하면 8KB짜리 버퍼를 16개 두게 됩니다.
  - 기본 설정 값은 `gzip_buffers 32 4k` 또는 `gzip_buffers 16 8k;` 입니다.
  - 기본 설정 값의 버퍼 크기는 메모리 페이지 1개의 크기와 같습니다.
- `gzip_http_version`
  - 응답을 압축하는 데 필요한 `request의 최소 HTTP 버전`을 설정합니다.
  - `1.0` 또는 `1.1`로 설정할 수 있습니다.
  - 기본 설정 값은 `gzip_http_version 1.1;` 입니다.
- `gzip_min_length`
  - gzip 압축을 진행할 `응답의 최소 크기` 를 지정하는 옵션입니다.
  - `length` 는 gzip 압축을 진행할 `응답의 최소 크기 (바이트)` 를 의미합니다.
  - `Content-Length 응답 헤더필드 값`과 `length`를 비교하여 더 클 경우에만 gzip 압축합니다.
  - 기본 설정 값은 `gzip_min_length 20;` 입니다.
  - **너무 작은 파일을 압축할 경우 발생하는 비효율을 없애기 위한 설정입니다.**
- `gzip_proxied`
  - Proxy를 통해서 들어오는 요청에 대해서도 gzip 압축 후, 응답할 것인지에 대한 설정입니다.
  - Proxy를 통해서 들어오는 요청을 구분하는 기준은 `요청 헤더필드 Via` 가 있는지 없는지로 구분합니다.
  - 가질 수 있는 값은 [참고자료](https://nginx.org/en/docs/http/ngx_http_gzip_module.html#gzip_proxied)를 확인합니다.
  - 기본 값은 `gzip_proxied off;` 입니다.
- `gzip_vary`
  - `응답헤더에 Vary: Accept-Encoding 필드 값`을 추가할지 말지를 설정합니다.
  - client 측에서 압축이 됐는지를 확인할 수 있게 합니다.
  - 기본 설정 값은 `gzip_vary off;` 입니다.

<br>

### Nginx Reverse Proxy 서버 그룹 구성 - [ngx_http_upstream_module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)

```nginx
upstream backend {
	server localhost:8080;
}
```

**`proxy_pass` 지시어의 대상이 되는 `upstream` 서버를 구성하기 위해 위와 같이 설정하였습니다.**

<br>

## /etc/nginx/conf.d/default.conf

[Nginx 최초 구성 파일](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#nginx-%EC%B4%88%EA%B8%B0-%EA%B5%AC%EC%84%B1%ED%8C%8C%EC%9D%BC---etcnginxconfddefaultconf) 과 **다른 내용에 대해서만 설명**을 하겠습니다.

```nginx
server {
    listen       80 default_server;
    server_name  localhost;
    
    location = / {
        root /usr/share/nginx/html;
        try_files /index.html /index.htm =404;
    }

    location = /index {
        root /usr/share/nginx/html;
        try_files /index.html /index.htm =404;
    }
    
    location = /favicon.ico {
        root  /usr/share/nginx/html;
        access_log off;
    }
    
    location = /response/text-file {
        return 200 "Success Response";
    }

    location = /redirect/external {
        return 307 http://www.naver.com;
    }
    
    location = /redirect/internal {
        return 308 internal-redirect;
    }
    
    rewrite_log on;
    location = /rewrite/flag-last {
        rewrite ^/rewrite/.*$ /index last;
    }
    
    location = /rewrite/flag-break {
        rewrite ^/rewrite/.*$ /index break;
    }
    
    location = /rewrite/flag-redirect {
        rewrite ^/rewrite/.*$ /index redirect;
    }
    
    location = /rewrite/flag-permanent {
        rewrite ^/rewrite/.*$ /index permanent;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
    
    location / {
        proxy_http_version 1.1;
		proxy_set_header Host $http_host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Content-Type $content_type;
        
        proxy_pass http://backend;
    }
}
```

<br>

### [`location` 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)

location 지시어는 request를 처리할 location 블록을 선택할 때, 사용되는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20location%20%EC%A7%80%EC%8B%9C%EC%96%B4.md

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

### [`return` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#return)

```nginx
location = /response/text-file {
    # "Success Response" 를 응답 body에 넣어서 전송합니다.
    # 기본 MiME 유형이 application/octet-stream 이므로 파일로 다운로드를 하게 됩니다.
    return 200 "Success Response";
}

location = /redirect/external {
    # 브라우저로 307 응답코드를 전송하여 http://www.naver.com 으로 외부 redirect를 수행하도록 합니다.
    # 결과적으로 브라우저에서 /redirect/internal-redirect 를 재호출합니다.
    return 307 http://www.naver.com;
}

location = /redirect/internal {
    # 브라우저로 308 응답코드를 전송하여 현재 path인 /redirect 에서 internal-redirect로 내부 redirect를 수행하도록 합니다.
    # 결과적으로 브라우저에서 /redirect/internal-redirect 를 재호출합니다.
    return 308 internal-redirect;
}
```

- `request의 처리`를 중지하고 지정된 응답코드를 client에 반환합니다.
- `code` 는 client에 반환되는 응답코드를 의미합니다.
- `text` 는 client에 반환되는 응답 body 문구를 의미합니다.
- client에 응답을 직접하는 것이 아니고 리다이렉트 기능을 사용하려면 `code`와 `URL`을 함께 명시합니다.
  - 리다이렉트 시, 사용가능한 `code` 값은 301, 302, 303, 307, 308 입니다.
  - `code` 값을 생략할 경우 기본 응답코드는 302가 됩니다.
  - `URL` 은 리다이렉트할 URL을 의미합니다.
  - `URL` 값은 `http://` 또는 `https://` 또는 `$scheme` 로 시작해야됩니다.
  - 리디렉션 `URL` 을 이 `서버의 로컬 URI`로 지정할 수 있습니다. 이 때, 실제 리디렉션 URL은 `$scheme` , `server_name_in_redirect`, `port_in_redirect` 지시문에 따라 구성됩니다.

**nginx 단에서 응답을 처리하는 동작에 대한 테스트를 위해 추가하였습니다.**

<br>

### [`rewrite` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite)

```nginx
location = /rewrite/flag-last {
    rewrite ^/rewrite/.*$ /index last;
}

location = /rewrite/flag-break {
    rewrite ^/rewrite/.*$ /index break;
}

location = /rewrite/flag-redirect {
    rewrite ^/rewrite/.*$ /index redirect;
}

location = /rewrite/flag-permanent {
    rewrite ^/rewrite/.*$ /index permanent;
}
```

- 지정된 `정규표현식`이 `요청 URI`와 매칭되면 `요청 URI`가 `대체 문자열`로 변경됩니다.

- **`return 지시어 + 3xx 응답코드` 와의 차이점은 현재 `request의 URI`를 재작성하여 `다른 URI`로 바꾸어 다른 location이 탐색되도록 한다는 것입니다. 그래서, 브라우저에 노출되는 URI는 전혀 변경되지 않습니다.  하지만, flag의 값으로 `redirect`나 `permanent`를 설정하면 redirect가 발생하여 `return 지시어 + 3xx 응답코드`와 동일한 효과를 냅니다.**

- `regex` 는 요청 URI와 매칭을 수행할 `정규표현식`을 의미합니다.

- `replacement` 는 `정규표현식`이 `요청 URI`와 매칭되면 `요청 URI`를 대체할 `대체 문자열`을 의미합니다.

- `flag` 지시어는 아래의 값 중 하나를 가질 수 있습니다.

  - `last` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`, 다음 처리는 중단하고 변경된 URI와 매칭되는 새 `location` 검색을 시작합니다.
  - `break` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`, 다음 처리는 중단합니다.
  - `redirect` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`, 다음 처리는 중단하고 변경된 URI와 함께 `일시적인 redirect를 뜻하는 응답코드 302`를 반환합니다.
  - `permanent` 로 설정하면 `정규표현식과 매칭될 때, 문자열 변경을 수행 후`, 다음 처리는 중단하고 변경된 URI와 함께 `영구적인 redirect를 뜻하는 응답코드 301`를 반환합니다.
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

**nginx 단에서 return 지시어와 rewrite 지시어의 동작 차이에 대한 테스트를 위해 추가하였습니다.**

**`flag` 값을 `last` 로 하였기 때문에 `localhost/rewrite/url` 를 호출하면 브라우저에서 URI는 변경되지 않지만, 실제 응답 값은 `localhost/index` URI의 응답이 반환됩니다.**

<br>

### [`rewrite_log` 지시어](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html#rewrite_log)

```nginx
rewrite_log on;
```

- `ngx_http_rewrite_module 모듈 지시어`의 처리 결과를 `error_log`에 `notice 레벨`로 로깅하는 기능을 활성화 또는 비활성화합니다.
- 기본 설정 값은 `rewrite_log off;` 입니다.

**nginx 단에서 return 지시어와 rewrite 지시어의 동작 차이에 대한 테스트를 위해 추가하였습니다.**

<br>

### [`proxy_http_version` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_http_version)

```nginx
proxy_http_version 1.1;
```

- 프록시를 위한 HTTP 프로토콜 버전을 설정합니다.
- Keepalive 연결 및 NTLM 인증기능을 사용하기 위해서 버전 **1.1을 사용하는 것이 좋습니다.**
- 기본 설정 값은 `proxy_http_version 1.0;` 입니다.

**Keepalive 연결 및 NTLM 인증기능을 사용하기 위해서 버전 1.1을 사용하는 것이 좋습니다.**

<br>

### [`proxy_set_header` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header)

```nginx
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Content-Type $content_type;
```

- `proxy_set_header 지시어`는 프록시 서버로 전달할 request 헤더 필드를 재정의하거나 추가할 수 있습니다.

- `field` 는 프록시 서버로 전달할 `헤더 필드의 이름` 을 의미합니다.
- `value` 는 프록시 서버로 전달할 `헤더 필드의 값` 을 의미합니다. `value` 값이 `""`와 같은 빈 문자열로 설정되면 프록시 서버로 헤더 필드가 전달되지 않습니다.
- 기본 설정 값은 `proxy_set_header Host $proxy_host; proxy_set_header Connection close;` 입니다.
- `request의 Host 헤더 필드`를 `$proxy_host` 값이 아닌 `원본 request의 Host 헤더 필드 값`을 사용하고 싶다면 다음과 같이 설정할 수 있습니다. `proxy_set_header Host $http_host;`
- `proxy_set_header 지시어` 가 없는 경우에만 이전 컨텍스트에 정의된 `proxy_set_header` 설정을 상속합니다.

**기본 설정 값은 프록시 서버로 헤더를 전달하지 않으므로 필요한 헤더 값을 프록시 서버로 전달하기 위해 위와 같이 설정하였습니다.**

<br>

### [`proxy_pass` 지시어](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass)

```nginx
proxy_pass http://backend;
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

**프록시 서버가 1대가 아닐 때, upstream 모듈의 기능을 활용한 프록시 서버 그룹 구성을 위해 위와 같이 설정하였습니다.**

<br>

### [최초 구성 파일에 존재하는 불필요한 설정](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#%EA%B7%B8-%EC%99%B8-%EB%B6%88%ED%95%84%EC%9A%94%ED%95%9C-%EC%84%A4%EC%A0%95) 은 제거