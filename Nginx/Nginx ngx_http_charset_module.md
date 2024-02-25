# Nginx ngx_http_charset_module

https://nginx.org/en/docs/http/ngx_http_charset_module.html

ngx_http_charset_module 모듈은 지정된 문자 세트를 응답 헤더 필드 "Content-Type" 에 추가합니다.

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