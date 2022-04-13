# CORS 개념, 이슈, 해결

CORS 오류에 대해서 학습한 것을 정리하였습니다.

## CORS(Cross Origin Resource Sharing) 개념

참고: [https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

다른 `Origin`(Cross Origin)에 서버 자원(Resource)을 공유(Sharing) 하도록 허용하는 정책을 의미

여기서, `Origin` 의 개념 (참고: [https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin))
origin = scheme + hostname + port
origin 예시: `https://local-www.domain.com:8081`

scheme, hostname, port 중에 하나라도 다르다면, `origin`이 다른 것으로 간주한다.
`https://alpha-www.domain.com` 에서 `https://alpha-remote.domain.com`으로 HTTP 요청을 보낼 경우, `Cross Origin`(다른 Origin으로 건너뛰는) 요청이 된다.

<br>

## CORS 이슈

참고: [https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

별도의 서버 설정이 없다면, CORS 정책은 폐쇄적으로 설정되어있다.
즉, 어떠한 설정도 없다면, 다른 `Origin` (Cross Origin)에서 온 HTTP 요청에 대해 응답을 하지 않는다.

다른 `Origin` 클라이언트에서 온 HTTP 요청을 허용하고 싶다면,
요청을 받는 서버의 `Access-Control-Allow-Origin`헤더 값으로 HTTP 요청을 허용할 `Origin`을 설정하면 된다.

`https://alpha-www.domain.com` 에서 `https://alpha-remote.domain.com`으로 HTTP 요청을 보내서, 특정 서버자원에 접근하고 싶다고 가정하자.

`https://alpha-remote.domain.com` 페이지를 제공하는 서버의 응답 헤더를 아래와 같이 설정해주면 된다.
`Access-Control-Allow-Origin: https://alpha-www.domain.com`

와일드 카드문자 \*을 사용하면, 모든 다른 `Origin` 에 대해 HTTP 요청을 허용한다.
`Access-Control-Allow-Origin: *`

<br>

## CORS 이슈 해결방법

CORS 이슈를 해결하는 방법에는 아래와 같은 방법들이 있습니다.

1. 특정 헤더를 포함한 요청에 대해서 CORS를 허용
2. 특정 Credential을 포함한 요청에 대해서 CORS를 허용
3. 특정 Origin에서 보낸 요청에 대해서 CORS를 허용

여기서는, 3의 관점으로 해결하는 방법을 주로 다루겠습니다.
`Access-Control-Allow-Origin` 헤더 값을 설정하는 방법에는 여러가지가 있다.

<br>

### 1\. 웹 서버 단에서 설정

`Nginx` 기준
server context 하위에 아래 구문 작성, 기능은 다음과 같습니다.

* HTTP 요청 헤더의 `origin` 값에 `domain.com`이라는 문구를 포함한다면, (nginx 정규식 문법으로 조건을 설정)
* HTTP 응답 헤더에 `Access-Control-Allow-Origin` 헤더를 추가하는 구문 작성

``` conf
server {
    ...
    if ($http_origin ~* (.*\.domain.com.*)) {
        add_header Access-Control-Allow-Origin "$http_origin";
    }
}
```

<br>

### 2\. Spring Annotation 이용

Controller의 @RequestMapping(@GetMapping, @PostMapping 등 포함) 메소드에
`@CrossOrigin` 애노테이션을 선언

* origins: HTTP 요청을 허용할 외부 `Origin`값 들을 설정
* 그 외에도 originPatterns, methods, allowedHeaders, exposedHeaders, maxAge, allowCredentials 값을 설정하여 HTTP 요청 허용 조건을 설정할 수 있음
* 참고: [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CrossOrigin.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/CrossOrigin.html)

``` java
@CrossOrigin(origins = {"https://local-www.domain.com:8081", "https://alpha-www.domain.com", "https://beta-www.domain.com", "https://www.domain.com"})
@GetMapping("/remote/test")
public String remoteTest(...) {
    ...
}
```

<br>

### 3\. Spring Configuration 설정

아래와 같이 Configuration 파일을 Bean으로 생성 후,
`CorsRegitry` 의 메소드에 요청허용 조건을 명세

* addMapping: 허용할 HTTP 요청 URI
* allowedOrigins: 허용할 외부 `Origin` 값들
* 참고: [https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/CorsRegistry.html](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/CorsRegistry.html)

``` java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:8080");
    }
}
```

<br>

## 참고자료

* [https://developer.mozilla.org/ko/docs/Web/HTTP/CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
* [https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Origin)
* [https://spring.io/guides/gs/rest-service-cors/](https://spring.io/guides/gs/rest-service-cors/)
* [https://www.baeldung.com/spring-cors](https://www.baeldung.com/spring-cors)
* [https://howtodoinjava.com/spring5/webmvc/spring-mvc-cors-configuration/](https://howtodoinjava.com/spring5/webmvc/spring-mvc-cors-configuration/)