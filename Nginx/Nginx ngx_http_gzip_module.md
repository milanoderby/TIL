# Nginx ngx_http_gzip_module

https://nginx.org/en/docs/http/ngx_http_gzip_module.html

ngx_http_gzip_module 모듈은 “gzip” 방식을 사용하여 응답을 압축하는 필터입니다.

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