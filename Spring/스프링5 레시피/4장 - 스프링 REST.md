# 4장 - 스프링 REST

### REST의 정의



### 웹 서비스



<br>

### AJAX

비동기 Javascript and Xml

<br>

### Payload와 Endpoint

Endpoint는 API 요청을 받아서 처리하는 말단 (Controller의)

페이로드는 API 요청을 할 때, 함께 전달되는 데이터

<br>

### Marshalling

 한 객체의 표현방식을 저장 또는 전송에 적합한 다른 데이터 형식으로 변환하는 과정입니다.

우리가 데이터를 API의 응답을 할 때, XML 또는 JSON으로 마샬링 하는 것입니다.

<br>

### Content-Type 과 Accept 헤더의 차이점

Accept Header는 Client가 어떤 데이터 형태를 원하는지 Server에게 알려주는 것

Content-Type은 API의 사용하는 데이터 형태와, 응답하는 데이터 형태를 명시하는 것입니다.

<br>

### HttpMessageConverter

```java
private final List<HttpMessageConverter> httpMessageConverter;
```

위를 선언하면, Spring 에서 2가지 HttpMessageConverter 구현체를 주입한다는 것을 알 수 있습니다.

1. StringMessageConverter
2. MappingJackson~

https://ict-nroo.tistory.com/134



<br>

### SEO

검색엔진이 검색을 

<br>

### @InitBinder

Controller 로 들어오는 요청에 대해 

책에서는 `Date` 객체를 



@ControllerAdvice에 @InitBinder를 선언하면, 여러 Controller에 전역적으로 적용할 수 있습니다.

<br>

### CustomDateEditor

PropertyEditor

PropertyEditorSupport: PropertyEditor를 구현한 클래스

CustomDateEditor는 PropertyEditorSupport를 상속한 클래스



<br>

### HttpResponseCode

Http 응답코드: 요청에 대해서, 



<br>

### Response Entity

일반적으로 

Http 응답코드를 지정하여 데이터를 반환할 때, 사용하는 객체입니다.



### WADL - Web Application Language

Payload의 구조를 나타내는 언어

예시로, swagger, spring docs 등이 있습니다.