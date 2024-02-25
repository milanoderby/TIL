# Nginx 구성 예시 - API 서버

API 서버를 위한 Nginx 구성 파일을 작성합니다.

## /etc/nginx/nginx.conf 파일

[Nginx 최초 구성 파일](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#nginx-%EC%B4%88%EA%B8%B0-%EA%B5%AC%EC%84%B1%ED%8C%8C%EC%9D%BC---etcnginxnginxconf) 과 **다른 내용에 대해서만 설명**을 하겠습니다.

```nginx
load_module "/etc/nginx/modules/ngx_http_geoip_module.so";

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
    
    geoip_country /etc/nginx/geoip/GeoLiteCountry.dat;
    geoip_city /etc/nginx/geoip/GeoLiteCity.dat;

    log_format json_format escape=json '{'
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
        '}, '
        '"geoip_country": { '
            '"geoip_country_code": "$geoip_country_code", '
            '"geoip_country_code3": "$geoip_country_code3", '
            '"geoip_country_name": "$geoip_country_name" '
        '}, '
        '"geoip_city": { '
            '"geoip_city": "$geoip_city", '
            '"geoip_region_name": "$geoip_region_name", '
            '"geoip_postal_code": "$geoip_postal_code", '
            '"geoip_latitude": "$geoip_latitude", '
            '"geoip_longitude": "$geoip_longitude" '
        '} '
    '}';

    access_log  /var/log/nginx/nginx-access.log  json_format;

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

### server_tokens 지시어

``` nginx
server_tokens off;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#server_tokens-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### charset 지시어

```nginx
 charset utf-8;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_charset_module.md#charset-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### log_format 지시어

```nginx
log_format json_format escape=json '{'
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
    '}, '
    '"geoip_country": { '
        '"geoip_country_code": "$geoip_country_code", '
        '"geoip_country_code3": "$geoip_country_code3", '
        '"geoip_country_name": "$geoip_country_name" '
    '}, '
    '"geoip_city": { '
        '"geoip_city": "$geoip_city", '
        '"geoip_region_name": "$geoip_region_name", '
        '"geoip_postal_code": "$geoip_postal_code", '
        '"geoip_latitude": "$geoip_latitude", '
        '"geoip_longitude": "$geoip_longitude" '
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
      },
      "geoip_country": {
          "geoip_country_code": "$geoip_country_code",
          "geoip_country_code3": "$geoip_country_code3",
          "geoip_country_name": "$geoip_country_name"
      },
      "geoip_city": {
          "geoip_city": "$geoip_city",
          "geoip_region_name": "$geoip_region_name",
          "geoip_postal_code": "$geoip_postal_code",
          "geoip_latitude": "$geoip_latitude",
          "geoip_longitude": "$geoip_longitude"
      }
  }
  ```

- 위에서 사용되는 변수는 각각 아래 항목에서 확인 가능합니다.
  - [http core 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_log_module.md#log_format-%EC%A7%80%EC%8B%9C%EC%96%B4)
  - [http proxy 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_proxy_module.md#ngx_http_proxy_module-%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
  - [http upstream 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_upstream_module.md#ngx_http_upstream_module%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
  - [http geoip 모듈 관련 변수](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_geoip_module.md#ngx_http_geoip_module%EB%AA%A8%EB%93%88%EC%97%90%EC%84%9C-%EC%A7%80%EC%9B%90%ED%95%98%EB%8A%94-%EB%B3%80%EC%88%98)
  
- 자세한 내용은 아래 페이지에서 참고 바랍니다.
  
  https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_log_module.md#log_format-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### access_log 지시어

```nginx
access_log  /var/log/nginx/nginx-access.log  json_format;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_log_module.md#access_log-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### keepalive 관련 지시어

``` nginx
keepalive_timeout 0;
```

API 서버는 브라우저와 통신하는 서버와는 달리 매우 짧은 시간 동안 많은 요청을 처리하므로 keep-alive 연결이 필요하지 않습니다. 따라서, keep-alive 설정을 최소화하여 서버 리소스를 효율적으로 관리하도록 합니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#keepalive-%EA%B4%80%EB%A0%A8-%EC%A7%80%EC%8B%9C%EC%96%B4

**위 설정은 keep-alive 연결을 즉시 종료하는 것입니다. 이렇게 설정하면 서버가 매 요청마다 새로운 연결을 생성하므로 keep-alive 연결 유지를 위해 사용되는 서버 리소스(메모리)가 없게 됩니다.**

**브라우저와 통신하는 서버가 아니기 때문에 keep-alive 연결을 유지하지 않도록 구성하였습니다.**

<br>

### gzip 관련 지시어

```nginx
gzip on;
gzip_disable "msie6";
gzip_comp_level 5;
gzip_types application/json;
gzip_min_length 1024;
gzip_proxied any;
gzip_vary on;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_gzip_module.md#gzip-%EA%B4%80%EB%A0%A8-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### Nginx Reverse Proxy 서버 그룹 구성

```nginx
upstream backend {
	server localhost:8080;
}
```

**`proxy_pass` 지시어의 대상이 되는 `upstream` 서버를 구성하기 위해 위와 같이 설정하였습니다.**

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_upstream_module.md

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

### location 지시어

location 지시어는 request를 처리할 location 블록을 선택할 때, 사용되는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20location%20%EC%A7%80%EC%8B%9C%EC%96%B4.md

<br>

### try_files 지시어

```nginx
try_files file ... uri;
try_files file ... =code;
```

지정된 순서대로 파일이 있는지 확인하고, 가장 먼저 발견된 파일을 요청 처리에 사용하는 지시어입니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#try_files-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### return 지시어

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

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_rewrite_module.md#return-%EC%A7%80%EC%8B%9C%EC%96%B4

**nginx 단에서 응답을 처리하는 동작에 대한 테스트를 위해 추가하였습니다.**

<br>

### rewrite 지시어

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

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_rewrite_module.md#rewrite-%EC%A7%80%EC%8B%9C%EC%96%B4

**nginx 단에서 return 지시어와 rewrite 지시어의 동작 차이에 대한 테스트를 위해 추가하였습니다.**

**`flag` 값을 `last` 로 하였기 때문에 `localhost/rewrite/url` 를 호출하면 브라우저에서 URI는 변경되지 않지만, 실제 응답 값은 `localhost/index` URI의 응답이 반환됩니다.**

<br>

### rewrite_log 지시어

```nginx
rewrite_log on;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_rewrite_module.md#rewrite_log-%EC%A7%80%EC%8B%9C%EC%96%B4

**nginx 단에서 return 지시어와 rewrite 지시어의 동작 차이에 대한 테스트를 위해 추가하였습니다.**

<br>

### proxy_http_version 지시어

```nginx
proxy_http_version 1.1;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_proxy_module.md#proxy_http_version-%EC%A7%80%EC%8B%9C%EC%96%B4

**Keepalive 연결 및 NTLM 인증기능을 사용하기 위해서 버전 1.1을 사용하는 것이 좋습니다.**

<br>

### proxy_set_header 지시어

```nginx
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Content-Type $content_type;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_proxy_module.md#proxy_set_header-%EC%A7%80%EC%8B%9C%EC%96%B4-%EC%99%80-proxy_set_body-%EC%A7%80%EC%8B%9C%EC%96%B4

**기본 설정 값은 프록시 서버로 헤더를 전달하지 않으므로 필요한 헤더 값을 프록시 서버로 전달하기 위해 위와 같이 설정하였습니다.**

<br>

### proxy_pass 지시어

```nginx
proxy_pass http://backend;
```

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_proxy_module.md#proxy_pass-%EC%A7%80%EC%8B%9C%EC%96%B4

**프록시 서버가 1대가 아닐 때, upstream 모듈의 기능을 활용한 프록시 서버 그룹 구성을 위해 위와 같이 설정하였습니다.**

<br>

### [최초 구성 파일에 존재하는 불필요한 설정](https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20%EA%B5%AC%EC%84%B1.md#%EA%B7%B8-%EC%99%B8-%EB%B6%88%ED%95%84%EC%9A%94%ED%95%9C-%EC%84%A4%EC%A0%95) 은 제거