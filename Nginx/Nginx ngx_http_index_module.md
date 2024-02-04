# Nginx ngx_http_index_module

https://nginx.org/en/docs/http/ngx_http_index_module.html

<br>

모듈 `ngx_http_index_module`은 슬래시 문자(' `/`')로 끝나는 요청을 처리합니다. 

이러한 요청은 [ngx_http_autoindex_module](https://nginx.org/en/docs/http/ngx_http_autoindex_module.html) 및 [ngx_http_random_index_module](https://nginx.org/en/docs/http/ngx_http_random_index_module.html) 모듈 로도 처리할 수 있습니다.

<br>

### 예시 구성파일

```nginx
http {
    server {
        listen 80;
        
        location / {
            root /test/dir
            index index.html index.ftl;
        }
    }
}
```

<br>

위와 같이 구성할 경우, `http://localhost/` 요청이 들어올 경우, 아래 파일을 순차적으로 탐색합니다.

- `/test/dir/index.html` 파일
- `/test/dir/index.ftl` 파일
- 만약 위 2개 파일이 없다면, nginx의 기본 404페이지가 응답됩니다.  만약, `error_page 지시어` 로 지정한 페이지가 있다면, 해당 페이지로 응답됩니다.
