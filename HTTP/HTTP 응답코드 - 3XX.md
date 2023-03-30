# HTTP 응답코드 - 3XX

### 301 Moved Permanently

의미: 요청된 리소스가 Location 헤더에 지정된 URL로 **영구적으로** 이동되었음을 알려줍니다.

응답코드를 받을 때, HTTP Client들(브라우저, 검색엔진)은 목적에 따라 다르게 행동합니다.

응답코드를 받은 브라우저는 Location 헤더에 지정된 URL로 redirect 요청을 전송합니다.

응답코드를 받은 검색엔진은 Location 헤더에 지정된 URL로 **링크를 갱신합니다. (요청한 자원이 영구적으로 이동되었기 때문입니다.)**

<br>

주의사항:heavy_exclamation_mark: Spec상 redirect 요청을 보낼 때, 기존 요청에서 사용했던 HTTP 메소드와 요청 Body를 그대로 사용하여 다시 전송해야되지만, 모든 User-Agent가 이 Spec을 준수하지는 않습니다. 

redirect 요청을 보낼 때, 기존에 사용했던 HTTP 메소드를 사용하거나, HTTP GET 메소드를 사용합니다.

따라서, nginx의 Https Redirect Code로 301을 사용하면, 아래와 같이 동작할 수 있습니다.

예시

1. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTP 요청 전송

   POST /api/example HTTP/1.1

   Host: http://example.pay.naver.com

2. HTTP Client :arrow_left: nginx 에서 HTTPS 프로토콜을 사용하도록 redirect 응답

   HTTP/1.1 301 Moved Permanently

   Location: https://example.pay.naver.com/api/example

3. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTPS 요청 전송

   GET /api/example HTTP/1.1

   Host: https://example.pay.naver.com

기존에 HTTP POST 메소드를 사용하던 요청이 301 Moved Permanently 응답을 받은 후에는 HTTP GET메소드를 사용하여 요청을 재전송할 수 있습니다.

<br>

### 302 Found

#### 302 Found 는 요청한 자원이 일시적으로 이동되었음을 의미하는 것을 제외하면, 301 Moved Permanently 와 동일하게 동작합니다.

의미: 요청된 리소스가 Location 헤더에 지정된 URL로 **일시적으로** 이동되었음을 알려줍니다.

응답코드를 받을 때, HTTP Client들(브라우저, 검색엔진)은 목적에 따라 다르게 행동합니다.

응답코드를 받은 브라우저는 Location 헤더에 지정된 URL로 redirect 요청을 전송합니다.

응답코드를 받은 검색엔진은 Location 헤더에 지정된 URL로 **링크를 갱신하지 않습니다. (요청한 자원이 일시적으로 이동되었기 때문입니다.)**

<br>

주의사항:heavy_exclamation_mark: Spec상 redirect 요청을 보낼 때, 기존 요청에서 사용했던 HTTP 메소드와 요청 Body를 그대로 사용하여 다시 전송해야되지만, 모든 User-Agent가 이 Spec을 준수하지는 않습니다.

redirect 요청을 보낼 때, 기존에 사용했던 HTTP 메소드를 사용하거나, HTTP GET 메소드를 사용합니다.

따라서, nginx의 Https Redirect Code로 302를 사용하면, 아래와 같이 동작할 수 있습니다.

예시

1. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTP 요청 전송

   POST /api/example HTTP/1.1

   Host: http://example.pay.naver.com

2. HTTP Client :arrow_left: nginx 에서 HTTPS 프로토콜을 사용하도록 redirect 응답

   HTTP/1.1 302 Found Permanently

   Location: https://example.pay.naver.com/api/example

3. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTPS 요청 전송

   GET /api/example HTTP/1.1

   Host: https://example.pay.naver.com

기존에 HTTP POST 메소드를 사용하던 요청이 302 Found 응답을 받은 후에는 HTTP GET메소드를 사용하여 요청을 재전송할 수 있습니다.

<br>

### 308 Permanent Redirect

#### 308 Permanent Redirect 는 301 Moved Permanently 와 의미는 동일하나, 원래 요청에 사용된 HTTP 메소드와 요청 Body를 재사용하여 redirect 요청을 수행한다는 동작의 차이가 있습니다.

의미: 요청된 리소스가 Location 헤더에 지정된 URL로 **영구적으로** 이동되었음을 알려줍니다.

응답코드를 받은 브라우저는 Location 헤더에 지정된 URL로 redirect 요청을 전송합니다.

응답코드를 받은 검색엔진은 Location 헤더에 지정된 URL로 **링크를 갱신합니다. (요청한 자원이 영구적으로 이동되었기 때문입니다.)**

<br>

301 Moved Permanently 과의 차이점:heavy_exclamation_mark: : 301 Moved Permanently 응답코드를 받은 HTTP Client가 사용할 HTTP 메소드를 예측할 수 없는 반면, 308 Permanent Redirect 응답코드를 받은 HTTP Client가 사용할 HTTP 메소드를 예측할 수 있습니다.

예시

1. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTP 요청 전송

   POST /api/example HTTP/1.1

   Host: http://example.pay.naver.com

2. HTTP Client :arrow_left: nginx 에서 HTTPS 프로토콜을 사용하도록 redirect 응답

   HTTP/1.1 308 Permanent Redirect

   Location: https://example.pay.naver.com/api/example

3. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTPS 요청 전송

   POST /api/example HTTP/1.1

   Host: https://example.pay.naver.com

<br>

### 307 Temporary Redirect

#### 307 Temporary Redirect 는 302 Found 와 의미는 동일하나, 원래 요청에 사용된 HTTP 메소드와 요청 Body를 재사용하여 redirect 요청을 수행한다는 동작의 차이가 있습니다.

의미: 요청된 리소스가 Location 헤더에 지정된 URL로 **일시적으로** 이동되었음을 알려줍니다.

응답코드를 받은 브라우저는 Location 헤더에 지정된 URL로 redirect 요청을 전송합니다.

응답코드를 받은 검색엔진은 Location 헤더에 지정된 URL로 **링크를 갱신하지 않습니다. (요청한 자원이 일시적으로 이동되었기 때문입니다.)**

<br>

302 Found 과의 차이점:heavy_exclamation_mark: : 302 Found 응답코드를 받은 HTTP Client가 사용할 HTTP 메소드를 예측할 수 없는 반면, 307 Temporary Redirect 응답코드를 받은 HTTP Client가 사용할 HTTP 메소드를 예측할 수 있습니다.

예시

1. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTP 요청 전송

   POST /api/example HTTP/1.1

   Host: http://example.pay.naver.com

2. HTTP Client :arrow_left: nginx 에서 HTTPS 프로토콜을 사용하도록 redirect 응답

   HTTP/1.1 307 Temporary Redirect

   Location: https://example.pay.naver.com/api/example

3. HTTP Client :arrow_right: nginx 와 연결되어있는 서버로 HTTPS 요청 전송

   POST /api/example HTTP/1.1

   Host: https://example.pay.naver.com

<br>

### 303 See Other

의미: 요청된 리소스 자체에 연결하지 않고 Location 헤더에 지정된 URL (다른 페이지)로 redirect 하도록 알려줍니다.

응답코드를 받은 HTTP Client가 다른 페이지로 redirect 할 때, 사용하는 HTTP 메소드는 항상 **GET**입니다.

<br>

다음과 같은 File 업로드 상황에서 사용할 수 있습니다.

예시

1. HTTP Client :arrow_right: 서버로 File 업로드 요청 전송

   POST /file/upload HTTP/1.1

   Host: http://example.pay.naver.com

2. HTTP Client :arrow_left: File 업로드 결과페이지로 redirect 하도록 응답

   HTTP/1.1 303 See Other

   Location: https://example.pay.naver.com/file/upload/result/{hash}

3. HTTP Client :arrow_right: File 업로드 결과페이지로 redirect 요청 전송

   GET /file/upload/result/{hash} HTTP/1.1

   Host: https://example.pay.naver.com

<br>

이를 표로 정리하면 아래와 같습니다.

| HTTP 응답코드          | 의미                                                         | 응답코드를 받을 때, 브라우저의 행동                          | 응답코드를 받을 때, 검색엔진의 행동                          | redirect 요청 시, 사용하는 HTTP 메소드                |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- |
| 301 Moved Permanently  | 요청된 리소스가 Location 헤더에 지정된 URL로 **일시적으로** 이동되었음 | 요청된 리소스가 Location 헤더에 지정된 URL로 redirect 요청을 보냅니다. | 요청된 리소스가 Location 헤더에 지정된 URL로 **링크를 갱신하지 않습니다.** | 기존 요청 시, 사용한 HTTP 메소드 또는 HTTP GET 메소드 |
| 302 Found              | 요청된 리소스가 Location 헤더에 지정된 URL로 **영구적으로** 이동되었음 | 요청된 리소스가 Location 헤더에 지정된 URL로 redirect 요청을 보냅니다. | 요청된 리소스가 Location 헤더에 지정된 URL로 **링크를 갱신합니다.** | 기존 요청 시, 사용한 HTTP 메소드 또는 HTTP GET 메소드 |
| 308 Permanent Redirect | 요청된 리소스가 Location 헤더에 지정된 URL로 **영구적으로** 이동되었음 | 요청된 리소스가 Location 헤더에 지정된 URL로 redirect 요청을 보냅니다. | 요청된 리소스가 Location 헤더에 지정된 URL로 **링크를 갱신합니다.** | 기존 요청 시, 사용한 HTTP 메소드                      |
| 307 Temporary Redirect | 요청된 리소스가 Location 헤더에 지정된 URL로 **일시적으로** 이동되었음 | 요청된 리소스가 Location 헤더에 지정된 URL로 redirect 요청을 보냅니다. | 요청된 리소스가 Location 헤더에 지정된 URL로 **링크를 갱신하지 않습니다.** | 기존 요청 시, 사용한 HTTP 메소드                      |
| 303 See Other          | 요청된 리소스 자체에 연결하지 않고 다른 페이지로 redirect 연결할 것 | 요청된 리소스가 Location 헤더에 지정된 URL로 redirect 요청을 보냅니다. |                                                              | HTTP GET 메소드                                       |

<br>

### 참고자료

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/303

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/307

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/308
