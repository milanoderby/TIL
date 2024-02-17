### [Nginx `location` 지시어](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)

location 지시어는 `요청 URI`에 따라 `수행할 구성 명령어를 설정`하는 블록 지시어입니다.

### location 지시어의 형태

`location 지시어`는 아래와 같은 형태로 명시할 수 있습니다.

```nginx
location [수정자] URI
```

<br>

예시는 아래와 같습니다.

```nginx
# 1번
location = /page1 {
    # URI가 정확히 /page1 일 때, location과 매칭이 됩니다.
}

# 2번
location /page2 {
    # URI가 /page2 로 시작하는 문자열일 때, location과 매칭이 됩니다.
    # 매칭이 되는 URI 예시: /page2-test, /page2/test
    # 3번 location과의 차이점은 아래 설명을 참고합니다.
}

# 3번
location ^~ /page3 {
    # URI가 /page3 로 시작하는 문자열일 때, location과 매칭이 됩니다.
    # 매칭이 되는 URI 예시: /page2-test, /page2/test
    # 2번 location과 다른 점은 아래 설명을 참고합니다.
}

# 4번
location ~ ^/[a-z]/test+$ {
    # URI가 명시된 정규표현식과 매칭되는 문자열일 때, location과 매칭이 됩니다.
    # 매칭이 되는 URI 예시: /page/test
    # 4번 location은 URI의 대소문자를 구분하는 반면, 5번 location과 URI의 대소문자를 구분하지 않는다는 점에서 다릅니다.
}

# 5번
location ~* ^/[a-z]/test+$ {
    # URI가 명시된 정규표현식과 매칭되는 문자열일 때, location과 매칭이 됩니다.
    # 매칭이 되는 URI 예시: /page/test, /page/TEST
    # 4번 location은 URI의 대소문자를 구분하는 반면, 5번 location과 URI의 대소문자를 구분하지 않는다는 점에서 다릅니다.
}

또는

# 6번
location @name {
	# URI와 매칭되지 않습니다.
    # error_page 의 파라미터로 이 location이 명시하면, 이 블록의 구성이 실행됩니다.
}
```

<br>

`location 지시어 예시` 에 대한 자세한 설명은 아래와 같습니다.

1. 수정자가 `=` 인 형태
   - `location의 파라미터 URI`와 `request의 URI`가 동일할 때 매칭이 됩니다.
   - `=` 뒤에는 `request uri`를 완전히 매칭시킬 문자열을 명시합니다.
   - `request의 URI`가 `=` 뒤에 붙은 `문자열`과 일치하면 바로 최종 매칭됩니다.
2. 수정자가 `없음` 또는 `^~`인 형태
   - `location의 파라미터 URI`가 `request의 URI`의 접두사(prefix)일 때 매칭이 됩니다.
   - 하지만, 매칭이 된다고 해서, 최종적으로 해당 location의 구성이 사용되는 것은 아닙니다. 사용될 location을 선정하는 방식은 다음 절에서 확인합니다.
3. 수정자가 `~` 또는 `~*`인 형태
   - `location의 파라미터 정규표현식`이 `request의 URI`와 매칭되는 문자열일 경우, 매칭이 됩니다.
   - `~` 뒤에는 대소문자를 구분하는 정규표현식 문자열이 옵니다.
   - `~*` 뒤에는 대소문자를 구분하지 않는 정규표현식 문자열이 옵니다.
4. 수정자가 `@으로 시작하는 문자열`인 형태
   - 일반적으로 인입되는 request를 처리하기 위해 사용되는 구성이 아닙니다.
   - Nginx 내부에서 `error_page 404 = @name` 와 같이 내부 redirection에 대해 처리하는 location입니다.

<br>

### request 가 인입될 때, `사용될 location 구성`을 선정하는 방식

#### 전처리 과정

먼저, `location` 의 매칭을 하기 전에 다음의 전처리 과정을 수행합니다.

1. `%XX` 형태의 인코딩된 문자열을 디코딩합니다.
2. `.` 과 `..` 같이 상대 경로를 풀어서 절대 경로로 변경합니다.
3. 2개 이상 인접해있는 슬래시 문자열(ex: `//`)를 하나의 슬래시 문자(`/`)로 변환합니다.

<br>

request 가 nginx로 인입된 경우, request의 URI와 매칭되는 location 을 찾는 방법은 아래와 같습니다.

1. 먼저, location의 수정자가 `=` 인 형태를 탐색합니다.

   - **`request의 URI`가 `location의 URI 파라미터`와 완전히 동일할 경우, 탐색을 종료하고 매칭된 location의 구성을 사용합니다.**

2. 두 번째로,  location의 수정자가 `없음` 또는 `^~` 인 형태를 탐색합니다.

   - `request의 URI의 접두사(prefix)`가 `location의 URI 파라미터` 인지 확인합니다.

   - 매칭되는 location 블록이 여러 개일 경우, **`location의 URI 파라미터` 가 가장 긴 location 블록**을 기억해둡니다. 
     - **만약, 이 location 블록의 수정자가 `^~` 인 경우, 탐색을 종료하고 매칭된 location의 구성를 사용합니다.** 
     - 아니라면, 아래 탐색 과정을 수행합니다.

3. 세 번째로, location의 수정자가 `~` 또는 `~*`인 형태를 탐색합니다.

   - nginx 설정파일의 위에서 아래방향으로 `location의 파라미터인 정규표현식`을 탐색합니다. 
   - **`request의 URI`와 `location의 파라미터인 정규표현식`이 매칭되는 블록이 있다면, 탐색을 종료하고 매칭된 location의 구성을 사용합니다.**
   - 아니라면, 2번 과정에서 기억해두었던 **`location의 URI 파라미터` 가 request URI의 prefix이면서 매칭되는 것 중 가장 긴 location**의 구성을 사용합니다.

4. **위의 탐색 과정을 통해 매칭되는 location 블록이 없다면 nginx 404 오류를 응답합니다.**

<br>

### location 구성의 예외적 동작

만약, location의 prefix 문자열이 `/` 문자로 끝나고, location 절에서 요청이 [proxy_pass](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass), [fastcgi_pass](http://nginx.org/en/docs/http/ngx_http_fastcgi_module.html#fastcgi_pass), [uwsgi_pass](http://nginx.org/en/docs/http/ngx_http_uwsgi_module.html#uwsgi_pass), [scgi_pass](http://nginx.org/en/docs/http/ngx_http_scgi_module.html#scgi_pass), [memcached_pass](http://nginx.org/en/docs/http/ngx_http_memcached_module.html#memcached_pass), [grpc_pass](http://nginx.org/en/docs/http/ngx_http_grpc_module.html#grpc_pass) 등에 의해 처리될 경우, 특수한 처리가 수행됩니다.

예를 들어 `location /user/` 가 있다고 가정합니다.

location이 가지는 prefix 문자열에서 가장 끝 `/` 문자를 제외한 문자열인 `/user`가 request 로 들어올 경우, 원칙적으로 `/user/` 는 `/user`의 prefix 문자열이 아니기 때문에 해당 location의 flow를 거치지 않아야 합니다. 하지만, 들어온 request uri의 뒤에 `/` 문자를 붙여서 `/user/` 로 redirect 호출됩니다. (이 때, 응답되는 HTTP 코드는 301입니다.)

이러한 동작을 원하지 않는다면, `location = /user`와 같이 뒤에 `/` 문자를 뺀 location을 `=` 수정자와 함께 명시합니다. 즉, request uri가 `/user` 인 경우에는 `/user/`로 redirect가 발생하지 않도록 하는 것입니다.

```nginx
location /user/ {
    proxy_pass http://example1.com;
}

location = /user {
    proxy_pass http://example2.com;
}
```

<br>

### Nginx의 location 탐색 성능을 최적화하기 위한 방법

1. 자주 호출되는 request의 URI는 `location = uri` 형태로 만들고, nginx 설정 파일 가장 위에 배치하여 탐색이 early terminate 되도록 합니다.
2. 위의 방식과 비슷하게 `location ^~ ` 형태의 location 블록을 nginx 설정 파일의 위쪽에 배치하여 탐색이 early terminate 되도록 합니다. 이 때, 문자열의 길이는 `location uri` 에서 사용되는 prefix 문자열의 길이보다는 무조건 길어야됩니다.
3. 그 외의 `location uri`, `location ~ uri`, `location ~* uri` 형태의 location 블록은 어차피 모든 탐색과정을 거치게 되므로 성능에 더 좋은 배치는 없습니다. 다만, request uri와 매칭되는 location 블록이 어떤 것인지 빠르게 파악하는 배치는 있을 수 있습니다. 이를 아래에서 확인해봅니다.

<br>

### Nginx location 구성의 가독성을 최적화하기 위한 방법

request의 URI가 어떤 location 과 매칭되는지 빠르게 확인하기 위해서는 nginx 설정 파일 상 가장 위에서 매칭되는 경우, 바로 탐색이 종료되는 순서로 배치합니다.

1. `location = uri` 은 가장 위에 배치합니다. -> 정확하게 동일한 request uri가 들어올 경우, 바로 탐색을 종료합니다.
2. `location ^~ uri` 은 두 번째 순서로 배치합니다. -> request uri의 문자열과 location의 prefix 문자열이 매칭될 경우, 바로 탐색을 종료합니다. 이 때, 문자열의 길이는 `location uri` 에서 사용되는 prefix 문자열의 길이보다는 무조건 길어야됩니다. 만약, `location /request` 와 `location ^~ req` 가 있을 경우, early terminate가 발생하지 않을 뿐만 아니라 절대로 매칭될 일이 없어지기 때문에 구문의 존재의미 자체가 없어집니다.
3. `location ~ uri`와 `location ~* uri`를 배치합니다. -> request uri의 문자열과 location의 정규표현식이 매칭될 경우, 바로 탐색을 종료합니다. 이 때,각 정규표현식이 교집합이 형성되지 않도록 주의합니다. 만약, request uri의 문자열이 `location ~ 정규표현식1` 과 `location ~ 정규표현식2` 에 모두 매칭될 경우, 둘 중 더 상위에 위치한 location 설정의 flow를 거치게 됩니다. nginx 수정 작업 중 두 설정의 위치를 바꾸게 된다면 기존의 방식대로 동작하지 않기 때문에 크게 주의합니다.
4. `location uri`를 prefix 문자열 길이가 긴 순서대로 배치합니다. -> request uri의 문자열과 location의 prefix 문자열이 매칭될 경우, 바로 탐색을 종료하는 것과 동일한 결과가 발생합니다. 실질적으로는 모든 location을 탐색하지만, prefix 문자열의 길이가 긴 순서대로 배치하였기 때문에 가장 위쪽에서 매칭되는 prefix 문자열을 가진 location의 flow를 거치게 됩니다.
5. `location /`를 배치합니다. -> 위에서 매칭되지 않은 모든 request uri와 매칭되도록 prefix 문자열을 설정하는 구문을 맨 아래에 추가합니다. 이 location 문까지 매칭되지 않는다면, 해당 요청은 처리되지 않는 문제가 발생하기 때문입니다.

<br>

### Nginx location 구성 예시

```nginx
location = / {
    [ configuration A ]
}

location ^~ /images/ {
    [ configuration D ]
}

location ~* \.(gif|jpg|jpeg)$ {
    [ configuration E ]
}

location /documents/ {
    [ configuration C ]
}

location / {
    [ configuration B ]
}
```

<br>

- request uri가 "/" 일 경우, configuration A 를 거치게 됩니다.
- request uri가 "/index.html" 일 경우, configuration B 를 거치게 됩니다.
- request uri가 "/documents/document.html” 일 경우, configuration C 를 거치게 됩니다.
- request uri가 "/images/1.gif" 일 경우, configuration D 를 거치게 됩니다.
- request uri가 "/documents/1.jpg" 일 경우, configuration E 를 거치게 됩니다.