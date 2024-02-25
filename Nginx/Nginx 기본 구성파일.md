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

### user 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#user-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### worker processes 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#worker-processes-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### error_log 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#error_log-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### pid 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#pid-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### worker_connections 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#worker_connections-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### use 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#use-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### include 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_core_module.md#include-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### default_type 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#default_type-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### server_tokens 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#server_tokens-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### charset 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_charset_module.md#charset-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### log_format 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_charset_module.md#charset-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### access_log 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_log_module.md#access_log-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### sendfile 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#sendfile-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### keepalive 관련 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#keepalive-%EA%B4%80%EB%A0%A8-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### gzip 관련 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_gzip_module.md#gzip-%EA%B4%80%EB%A0%A8-%EC%A7%80%EC%8B%9C%EC%96%B4

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

### root 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#root-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### alias 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#alias-%EC%A7%80%EC%8B%9C%EC%96%B4

<br>

### index 지시어

`index 지시어`는 `request의 URI`가 `/`로 끝났을 때, `root` 지시자 또는 `alias` 지시자와 `request 의 URI`가 결합된 파일 경로에 존재하는 기본 응답파일을 명시합니다.

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_index_module.md

<br>

### error_page 지시어

자세한 내용은 아래 페이지에서 참고 바랍니다.

https://github.com/milanoderby/TIL/blob/master/Nginx/Nginx%20ngx_http_core_module.md#error_page-%EC%A7%80%EC%8B%9C%EC%96%B4

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

