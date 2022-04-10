# Nginx 설정

* 이 글은 `nginx-1.18.0` 버전 기준으로 작성되었습니다.

<br>

## Nginx 설정

* nginx 설치 후, default로 설정되어 있는 `~/apps/nginx/conf/nginx.conf` 파일 내용 (각 버전별로 다를 수 있습니다.)

```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        # for NSight Nginx Monitoring
        location /server-status {
            extended_status on;
            allow 127.0.0.1;
            deny all;
            access_log off;
        }

        # for Tomcat Nginx Monitoring - If you don't need to monitoring Tomcat, remove this section
        location /managerAgent {
            allow 127.0.0.1;
            deny all;
            access_log off;
        }

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
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


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

위처럼 처음 셋팅에서는 대부분의 설정이 주석처리되어 있습니다.

각각의 설정 지시어(directive)와 값(value)을 customizing 하겠습니다.

1. `worker processes` 지시어 - [http://nginx.org/en/docs/ngx\_core\_module.html#worker\_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)

```
worker_processes auto;
```

* nginx에는 master process와 worker process가 있는데, worker processes는 말 그대로 Request를 처리하는 프로세스들을 의미합니다.
* worker processes의 개수는 CPU의 코어 개수와 맞춰주는 것이 성능상 좋습니다.
* 위와 같이 설정하면, worker process를 CPU 코어의 개수로 자동으로 설정합니다.

<br>
2. `worker_connections` 지시어 - [http://nginx.org/en/docs/ngx\_core\_module.html#worker\_connections](http://nginx.org/en/docs/ngx_core_module.html#worker_connections)

```
events {
    worker_connections  1024;
}
```

* 하나의 worker process는 여러 개의 Request와 Connection을 맺을 수 있습니다.
* worker\_connections는 하나의 worker process가 맺을 수 있는 Connection의 최대값을 설정합니다.
* 기본 값은 512이지만, 일반적인 서버는 더 많은 수의 Connection을 맺을 수 있습니다.
* `Nginx 서버가 처리할 수 있는 동시접속 수` = `worker_processes` X `worker_connections` 이므로 서버에 필요한 동시접속 트래픽을 고려하여 설정하도록 합니다. (테스팅 등 트래픽을 예측하는 작업이 필요합니다.)
* 이번 서버는 관리자를 위한 서버이기 때문에 1024로 설정하겠습니다.
* `worker_connections` 지시어는 events Context 안에서 설정합니다.

<br>
3. `server_tokens` 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_core\_module.html#server\_tokens](http://nginx.org/en/docs/http/ngx_http_core_module.html#server_tokens)

```
server_tokens off;
```

* default error page와 응답헤더의 `Server`필드에서 nginx 버전을 보여줄지 말지를 설정합니다.
* `server_tokens off` 로 설정하면, default error page와 응답헤더의 `Server`필드에서 nginx 버전을 생략합니다.

<br>
4. `charset` 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_charset\_module.html](http://nginx.org/en/docs/http/ngx_http_charset_module.html)

```
 charset utf-8;
```

* 응답헤더의 `Content-Type` 필드에 지정한 charset 을 추가하는 설정입니다.

<br>
5. `sendfile` 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_core\_module.html#sendfile](http://nginx.org/en/docs/http/ngx_http_core_module.html#sendfile)

```
sendfile on;
```

* 서버에서 커널함수인 `sendfile()` 함수 사용을 허용할지 말지 설정합니다.
* 일반적으로 파일을 전송할 때, `read()`, `write()` 과정을 거칩니다. 동일한 파일객체에 대해서 2번의 I/O 작업으로 context switch가 발생하는 것은 비효율적입니다. 앞의 2번의 system call 대신에, `sendfile()` 함수를 사용하면, 전송속도를 최적화할 수 있습니다. 복사 붙여넣기가 아니라 파일 캐시의 주소 값(pointer)을 소켓으로 바로 전송하는 원리입니다. (참조: [https://www.netguru.com/codestories/nginx-tutorial-performance](https://www.netguru.com/codestories/nginx-tutorial-performance)

<br>
5. `keepalive` 관련 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_core\_module.html](http://nginx.org/en/docs/http/ngx_http_core_module.html)

```
keepalive_timeout 65;
```

* keep-alive 란?
    * HTTP Connection은 `connectionless`(비연결성) 특성을 가지고 있습니다. 클라이언트 요청 시마다 매번 connection을 맺고, 응답을 완료하면, 연결을 끊어버립니다.
    * 위와 같은 특성으로 client가 매번 새로운 `연결/해제` 과정에서 오버헤드가 발생합니다.
    * 이러한 오버헤드를 줄이기 위해 `연결을 재사용하도록 유지하는 방식`이 keep-alive입니다.
    * keep-alive를 통해 client와 server의 connection을 유지한다는 것은 서버의 메모리 자원 또한 해제되지 않고, 유지됨을 의미합니다.
    * 그래서, `재사용횟수 제한`, `유지시간 제한`, `서버자원 제한`을 두어 keep-alive connection들을 주기적으로 해제합니다.
* `keepalive_requests`: 하나의 keep-alive connection에 대해서 최대 몇 번 재사용가능한지를 설정합니다. default 값은 100입니다. (`재사용횟수 제한`)
* `keepalive_timeout`: 하나의 keep-alive connection을 몇 초간 유지할지 설정합니다. default 값은 75s입니다. (`유지시간 제한`)
    * 너무 길게 유지하면, 서버자원(메모리)의 한계점까지 connection이 맺어져있을 경우, `keep-alive connection`으로 인해 새로운 request가 거부될 수 있습니다.
* `keepalive_requests`, `keepalive_timeout`는 http, server, location Context에서 설정가능합니다. - [http://nginx.org/en/docs/http/ngx\_http\_core\_module.html](http://nginx.org/en/docs/http/ngx_http_core_module.html)
* `keepalive`: Nginx 서버에서 최대로 유지할 Connection의 수를 설정합니다. 이 수를 넘어가면, LRU(Least Recently Used) 알고리즘에 의해 가장 오래 전에 맺어진 connection 부터 제거합니다. (`서버자원 제한`)
* `keepalive`는 upstream Context에서 설정가능합니다.- [http://nginx.org/en/docs/http/ngx\_http\_upstream\_module.html](http://nginx.org/en/docs/http/ngx_http_upstream_module.html)
* 이번 서버 설정에서는 위와 같이 설정하였습니다. 이유는, 클라이언트가 적어도 65초에 한번씩은 서버에 요청(페이지이동, ajax요청 등)을 할 것이라고 생각하여, connection 성립 이후, 지정한 시간동안은 connection 재사용을 위해 유지하도록 하였습니다.

<br>
6. `log_format` 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_log\_module.html#log\_format](http://nginx.org/en/docs/http/ngx_http_log_module.html#log_format)

```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
```

* log 형식을 특정 변수에 지정하는 설정입니다.
* 이번 서버설정에서는 default 로그 형식을 그대로 사용합니다.

<br>
7. `access_log` 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_log\_module.html#access\_log](http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log)

```
access_log  /home1/irteam/logs/nginx/nginx-1.18.0/instance_name/access.log  main;
```

* access log의 저장경로와 저장할 log형식을 지정합니다.
* 이번 서버설정에서는 access\_log 형식을 위에서 지정한 main `log_format`형식으로 기록되도록 설정하겠습니다.
* `instance_name` 디렉토리는 처음에는 없는 디렉토리이므로 mkdir 명령어로 생성해줍니다.
* `instance_name` 네이밍은 각 서버 인스턴스로 이름 수정합니다.

<br>

8. `gzip` 관련 지시어 - [http://nginx.org/en/docs/http/ngx\_http\_gzip\_module.html](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

```
gzip on;
gzip_disable "msie6";
gzip_comp_level 5;
gzip_types text.html text/css application/javascript application/json font/wotf font/wotf2 image/x-icon;
gzip_buffers 16 8k;
gzip_min_length 1024;
gzip_proxied any;
gzip_vary on;
```

* gzip 압축을 이용하면, 전송할 파일의 크기가 줄어 네트워크 전송속도를 향상시킬 수 있습니다.
* `gzip`: gzip압축을 사용할지 여부를 설정합니다.
* `gzip_disable`
    * Microsoft IE 4 \~ 6 버전에서는 압축을 지원하지 않기 때문에 gzip 압축대상에서 예외처리합니다.
    * `"msie6"` 라는 표현은 정규표현식으로 `"MSIE[4-6]\."` 와 일치합니다.
* `gzip_comp_level`
    * 압축레벨을 0 \~ 9 중 하나로 설정합니다.
    * 압축되는 크기와 CPU 자원사용량을 고려했을 때, 5레벨이 적절합니다.
    * 압축률 참조 - [https://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level](https://serverfault.com/questions/253074/what-is-the-best-nginx-compression-gzip-level)
* `gzip_types`
    * 지정한 MIME type들을 가지는 파일들에 대해서만 압축을 진행합니다.
    * 기본적으로 text/html 파일은 지정하지 않아도 압축의 대상이 됩니다.
* `gzip_buffers`
    * 버퍼를 두어서, 압축의 속도를 빠르게 할 수 있습니다.
    * 파일 전송 시, 버퍼를 사용하여 전송속도를 빠르게하는 원리와 비슷합니다.
    * 16 8k는 8KB짜리 버퍼를 16개 두겠다는 의미입니다.
* `gzip_http_version`
    * gzip\_http\_version의 기본값은 1.1로 설정되어 있기 때문에, 위 설정에는 추가하지 않았습니다.
* `gzip_min_length`
    * 특정 크기 이상의 파일에 대해서 gzip 압축을 진행한다.
    * default 값은 20, 기본 단위는 바이트
    * 너무 작은 파일을 압축할 경우 발생하는 비효율을 없애기 위한 설정
* `gzip_proxied`
    * Proxy를 통해서 들어오는 요청에 대해서도 gzip 압축 후, 응답할 것인지에 대한 설정
    * Proxy를 통해서 들어오는 요청을 구분하는 기준은 요청헤더에 "Via" 필드가 있는지 없는지로 구분한다.
    * 기본 값은 off 이다.
* `gzip_vary`
    * 응답헤더에 "Vary: Accept-Encoding" 필드를 넣을지 말지를 설정
    * 기본 값은 off이다.
    * 압축이 됐는지를 확인하기 위함

<br>
9\. 불필요한 기본 설정 제거

```
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
```

* PHP 관련 주석처리문 제거 - WebAppServer(WAS)로 SpringBoot 서버를 이용하기 때문에 아래 설정은 필요 없습니다. 이미 주석처리가 되어있지만, 혼동을 피하기 위해 제거합니다.
* htaccess file 접근 관련 설정도 제거합니다.

<br>

10. Reverse Proxy 서버 설정
    * Reverse Proxy를 사용하여 얻는 효과
        * `Security`: WAS는 DB에 직접 접근하는 서버이기 때문에, 외부에 노출되면, 시스템이 공격에 취약해집니다. 그렇기 때문에, 웹 서버를 가장 외부 단에 두어 시스템을 보호하는 역할을 수행하기 위함입니다.
        * `Load Balancing`: 클라이언트의 Request로 발생하는 부하(traffic)를 여러 대의 WAS에 적절하게 분산하기 위함입니다.
        * `Web Acceleration`: WAS까지 갈 필요 없는 정적자원 요청을 웹서버 단에서 처리하거나, 캐싱하여 성능을 향상하기 위함입니다.
        * 참조 - [https://www.nginx.com/resources/glossary/reverse-proxy-server/](https://www.nginx.com/resources/glossary/reverse-proxy-server/)
    * `server` 지시어: 각각의 요청을 처리할 가상서버를 설정합니다. - [http://nginx.org/en/docs/http/ngx\_http\_core\_module.html#server](http://nginx.org/en/docs/http/ngx_http_core_module.html#server)
    *

<br>
10\. proxy\.conf 설정 및 include

```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto http;
client_max_body_size 500m;
proxy_max_temp_file_size 0;
proxy_pass_request_headers on;
```

* proxy 서버로 요청을 전달할 때, 기존 요청 헤더 값도 전달해주어야 프록시 서버가 정상동작합니다.
    * 아래와 같은 설정이 없다면, 요청을 처리하고, 응답해줄 클라이언트를 찾을 수도 없게 되고, 각 요청헤더 값을 못받으면, 정상적인 요청처리가 불가합니다.
    * `proxy.conf` 파일을 생성해주고, 아래의 내용을 저장합니다.
* 그 후, 위의 내용을 그대로 include 해줍니다.

```
include       proxy.conf;
```

