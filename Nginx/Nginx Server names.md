## Nginx Server names

https://nginx.org/en/docs/http/server_names.html

<br>

## 서버명

### 개념

서버명은 `server_name 지시자` 의 파라미터로 request를 어떤 `server 블록`으로 전송할지 정하기 위해 사용되는 개념입니다.

서버명은 아래와 같은 형태로 지정될 수 있습니다.

- 정확한 이름
- 와일드카드 이름
- 정규표현식 이름

```nginx
server {
    listen       80;
    # 정확한 이름
    server_name  example.org  www.example.org;
    ...
}

server {
    listen       80;
    # 와일드카드 이름
    server_name  *.example.org;
    ...
}

server {
    listen       80;
    # 와일드카드 이름
    server_name  mail.*;
    ...
}

server {
    listen       80;
    # 정규표현식
    server_name  ~^(?<user>.+)\.example\.net$;
    ...
}
```

<br>

### 서버명을 이용하여 request를 전송할 가상 서버 결정 방법

`request의 Host 헤더 값`과 매칭되는 `서버명`이 여러 개일 경우, (예시: 와일드카드 이름과 정규표현식 모두 일치하는 경우 등) 아래 우선 순위대로 request를 전송할 가상 서버를 결정합니다.

1. 정확한 이름
2. 별표로 시작하는 가장 긴 와일드카드 이름(예: “ `*.example.org`”)
   - 만약, 2개의 와일드카드 이름이 매칭된다면, 더 길게 매칭되는 와일드카드 이름을 가지는 `server 블록`이 선택됩니다.
   - 위에서 매칭되는 와일드카드 이름의 길이 마저 동일하다면, 구성 파일에서 위에 명시된 `server 블록`이 선택됩니다.
3. 별표로 끝나는 가장 긴 와일드카드 이름(예: “ `mail.*`”)
   - 만약, 2개의 와일드카드 이름이 매칭된다면, 더 길게 매칭되는 와일드카드 이름을 가지는 `server 블록`이 선택됩니다.
   - 위에서 매칭되는 와일드카드 이름의 길이 마저 동일하다면, 구성 파일에서 위에 명시된 `server 블록`이 선택됩니다.
4. 첫 번째로 일치하는 정규 표현식(구성 파일에 나타나는 순서대로)

<br>

### 와일드카드 이름

와일드카드 이름은 이름의 시작이나 끝에만 별표(`*`)를 포함할 수 있습니다. 

가능한 와일드카드 이름

- `*.example.org` : 아래의 값과 매칭됩니다.
  - `www.example.org`
  - `www.sub.example.org`
- `.example.org` : 아래의 값과 매칭됩니다.
  - `www.example.org`
  - `www.sub.example.org`
  - `example.org`

<br>

불가능한 와일드카드 이름: 대신, 동일한 의미의 정규표현식으로는 지정 가능합니다.

- `www.*.example.org`
  - `~^www\..+\.example\.org$` : 대체할 수 있는 정규표현식 이름
- `w*.example.org`
  - `~^w.*\.example\.org$` : 대체할 수 있는 정규표현식 이름

<br>

### 정규표현식 이름

nginx에서 사용되는 정규표현식은 Perl 프로그래밍 언어(PCRE)에서 사용되는 정규표현식과 호환됩니다. 

**정규표현식을 사용하려면 서버명이 물결표 문자(`~`)로 시작해야 합니다.**

#### 주의사항

- 정규표현식 이름의 시작은 `^`로 명시하고, 정규표현식 이름의 시작은 `$`로 명시합니다. 문법적으로 필수는 아니지만, 논리적으로는 필수입니다.
- 정규표현식 이름에서 `.` 문자 자체를 표현할 때에는 `\.` 과 같이 `\`문자로 escape 처리해야합니다.
- 정규표현식 이름에서 수량자 `{}` 문법을 표현할 때에는 `"{"`, `"}"` 과 같이 `"`문자로 escape 처리해야합니다.
  - 틀린 문법 예시: `server_name  "~^(?<name>\w\d{1,3}+)\.example\.net$";`
  - 위 문법은 nginx에서 `{` `}` 문자가 `블록 지시자`로 해석되기 때문에 틀린 문법으로 처리되고, 다음과 같은 메세지가 출력됩니다. `directive "server_name" is not terminated by ";" in ...` 
  - 올바른 문법: `server_name  "~^(?<name>\w\d"{"1,3"}"+)\.example\.net$";`
  - 위와 같이 `"{"` 와 `"}"` 으로 `"` 문자로 감싸주어야 합니다.

<br>

#### 캡처 기능

캡처 기능이란 정규표현식에서 매칭되는 일부 문자열을 변수로 저장하기 위해 사용되며, 아래와 같은 문법을 사용합니다.

`(캡처될 정규표현식)`

- `( )`: 캡처할 정규표현식의 범위를 지정합니다.
- 정규표현식과 매칭되는 문자열을 저장할 변수가 번호로 저장됩니다.
- 정규표현식에서 캡처된 문자열의 순서대로 저장되며, `$1`, `$2` 변수를 사용하여 다른 지시문의 파라미터로 사용됩니다.

`(?<캡처할 변수명>캡처할 정규표현식)`

- `( )`: 캡처할 정규표현식의 범위를 지정합니다.
- `?<캡처할 변수명>`: 정규표현식과 매칭되는 문자열을 저장할 변수의 이름을 지정합니다.
- `$캡처할 변수명` 변수를 사용하여 다른 지시문의 파라미터로 사용됩니다.
- 가장 최신 PCRE 버전에서 사용되는 문법, nginx 버전이 오래 되었다면 다른 문법을 사용합니다. 이는 docs에서 직접 확인합니다.

<br>

예시는 아래와 같습니다.

```nginx
server {
    server_name   ~^(www\.)?(?<domain>.+)$;

    location / {
        root   /sites/$domain;
    }
}
```

<br>

`(www\.)`

- `( )`: 매칭할 정규표현식의 범위를 지정합니다.

- `www\.`: 실제 매칭되는 정규표현식입니다.
- `$1`: 캡처되는 순서 상 첫번째이므로 숫자 변수 `1`에 저장됩니다. 다른 지시문에서 `$1` 형태로 해당 문자열을 사용할 수 있습니다.

<br>

`(?<domain>.+)`

- `( )`: 매칭할 정규표현식의 범위를 지정합니다.

- `.+`: 실제 매칭되는 정규표현식입니다.

- `domain`: 위의 정규표현식과 매칭되는 문자열을 저장하는 변수의 이름입니다. 다른 지시문에서 `$domain` 형태로 해당 문자열을 사용할 수 있습니다.

<br>

### 기타 이름

특별하게 처리되도록 하는 서버명에 대해 알아봅니다.

#### 1. `request 의 Host 헤더 값`이 없을 경우, default_server 가 아닌 `server 블록`으로 request를 전송하려면, 아래와 같이 구성합니다.

`server_name 지시자`의 값으로 `""`을 사용합니다.

```nginx
server {
    # 80포트에 맵핑되는 것이 없을 경우, request를 처리하는 default server
    listen       80 default_server;
    server_name  example.com;
    ...
}

server {
    listen       80;
    server_name  example.org  www.example.org  "";
    ...
}
```

<br>

#### 2. `request 의 Host 헤더 값`이 서버의 호스트명일 경우, default_server 가 아닌 `server 블록`으로 request를 전송하려면, 아래와 같이 구성합니다.

`server_name 지시자`의 값으로 `$hostname` 변수를 사용합니다.

```nginx
server {
    # 80포트에 맵핑되는 것이 없을 경우, request를 처리하는 default server
    listen       80 default_server;
    server_name  example.com;
    ...
}

server {
    listen       80;
    server_name  example.org  www.example.org  $hostname;
    ...
}
```

<br>

#### 3. `request 의 Host 헤더 값`이 IP일 경우, default_server 가 아닌 `server 블록`으로 request를 전송하려면, 아래와 같이 구성합니다.

`server_name 지시자`의 값으로 `IP`값을 사용합니다.

```nginx
server {
    # 80포트에 맵핑되는 것이 없을 경우, request를 처리하는 default server
    listen       80 default_server;
    server_name  example.com;
    ...
}

server {
    listen       80;
    server_name  example.org  www.example.org  192.168.1.1;
    ...
}
```

<br>

#### 4. `request 의 Host 헤더 값`과 매칭되는 `server 블록`이 없을 경우, 오류 코드 444를 응답하려면, 아래와 같이 구성합니다.

`listen 지시자`의 값으로 `default_server`를 사용합니다.

`server_name 지시자`의 값으로 `_` 변수를 사용합니다. 

- 굳이 `_`가 아니어도 됩니다. client가 호출하지 않을 `Host 헤더의 값`이라면, `--` 또는 `!@#` 같은 값도 가능합니다.

```nginx
server {
    # 80포트에 맵핑되는 것이 없을 경우, request를 처리하는 default server
    listen       80 default_server;
    server_name  _;
    ...
}

server {
    listen       80;
    server_name  example.org  www.example.org;
    ...
}
```

<br>

### 최적화

#### server_name 해시테이블

listen 포트 별로 3개의 해시테이블이 있습니다. 

`server_name 지시자의 파라미터`는 각각 일치하는 해시테이블에 저장이 되며, 이 해시테이블은 `request의 Host 헤더 값`을 매칭할 때, 사용됩니다.

- 정확한 이름을 저장하는 해시테이블
- `*`로 시작하는 와일드카드 이름을 저장하는 해시테이블
- `*`로 종료하는 와일드카드 이름을 저장하는 해시테이블

<br>

#### server_name 해시테이블 탐색순서

`request의 Host 헤더 값`과 매칭되는 `server_name`이 있는지 아래 해시테이블 순서로 탐색합니다. 각 해시테이블에서 탐색되지 않으면, 다음 해시테이블을 탐색합니다.

1. 정확한 이름을 저장하는 해시테이블
2. `*`로 시작하는 와일드카드 이름을 저장하는 해시테이블
3. `*`로 종료하는 와일드카드 이름을 저장하는 해시테이블

<br>

#### server_name 해시테이블 탐색속도 및 탐색방법

탐색 속도는 아래 순서대로 빠릅니다.

1. 정확한 이름을 저장하는 해시테이블
2. `*`로 시작하는 와일드카드 이름을 저장하는 해시테이블, `*`로 종료하는 와일드카드 이름을 저장하는 해시테이블
   - `.` 문자로 나뉘어진 문자열 단위로 탐색하기 때문에 `정확한 이름 해시테이블` 보다는 탐색속도가 느립니다.
   - 예를 들어, `*로 시작하는 와일드카드 이름을 저장하는 해시테이블`에 `*.example.com`, `*.sub.example.com` 2가지 와일드카드 이름이 저장되어있다고 합시다.
   - `*로 시작하는 와일드카드 이름을 저장하는 해시테이블`에는 `sub.example.com`, `exaple.com` 형태로 저장되어 있을 것입니다.
   - `request의 Host 헤더 값`이 `first.second.sub.example.com` 이라고 할 때, 탐색하는 방법은 아래와 같습니다.
     1. 해시테이블에서 `first.second.sub.example.com`을 탐색합니다. 탐색이 되지 않기 때문에 다음 도메인 탐색을 수행합니다.
     2. 해시테이블에서 `second.sub.example.com`을 탐색합니다. 탐색이 되지 않기 때문에 다음 도메인 탐색을 수행합니다.
     3. 해시테이블에서 `sub.example.com`을 탐색합니다. 탐색 및 매칭되었기 때문에 해당 해시테이블의 bucket에 저장된 값인 `server 블록 주소`로 request를 전송합니다.
3. 정규표현식
   - 해시테이블에 저장될 수 없고, 순차적으로 모든 정규표현식과 매칭하는 방식이기 때문에 매우 느립니다.

<br>

#### 결론: 자주 호출되는 `server_name`은 `정규표현식`이나 `와일드카드 이름` 이 아닌 `정확한 이름`으로 명시하는 것이 효율적입니다.

비효율적인 방식 예시: 간결하기는 하지만, 비효율적입니다.

```nginx
server {
    listen       80;
    server_name  .example.org;
    ...
}
```

<br>

효율적인 방식 예시: 간결하지는 않지만, 자주 호출되는 `server_name`을 `정확한 이름` 방식으로 명시하여 더 빠른 해시테이블을 사용하기 때문에 더 효율적입니다.

```nginx
server {
    listen       80;
    server_name  example.org  www.example.org  *.example.org;
    ...
}
```

<br>

#### server_names_hash_max_size 와 server_names_hash_bucket_size

[server_names_hash_max_size](https://nginx.org/en/docs/http/ngx_http_core_module.html#server_names_hash_max_size) 지시자는 server_name 해시테이블이 저장할 수 있는 최대 server_name 개수를 명시합니다.

 [server_names_hash_bucket_size](https://nginx.org/en/docs/http/ngx_http_core_module.html#server_names_hash_bucket_size) 지시자는 server_name 해시테이블이 저장할 수 있는 server_name 의 최대 길이를 명시합니다.

- 이 지시자의 기본 값은 32입니다.
- 이 지시자의 값은 CPU 캐시 라인 크기에 맞춰줍니다. 그래서, 기본 값 server_name 길이가 32보다 크다면, 그 2배인 64로 변경합니다.

<br>

#### 특정 listen 포트에 바인딩된 `server 블록`이 유일한 경우

`server_name` 과 매칭되지 않아도 해당 `server 블록`이 default server가 되기 때문에 `server_name` 매칭 테스트를 수행하지 않습니다. 그래서, **listen 포트에 바인딩되는 해시테이블도 생성되지 않습니다.**

다만, 정규표현식의 캡처링 기능을 사용하면, 캡처링되는 문자열을 변수에 저장하기 위해 예외적으로 `server_name` 매칭 테스트가 수행됩니다.

<br>