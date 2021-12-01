# @Value 애노테이션

### 정의

@Value 애노테이션은 **Spring에 의해 관리되는 Bean** 안에서 **외부 Property (*.properties 나 *.yaml 형식)** 을 가져올 때, 사용하는 방식입니다. 

이 때, **가져온 Property 값**은 field나 method에 주입할 수 있습니다.

<br>

### 주의사항

여기서, Spring에 의해 관리되는 Bean이라는 제한사항은 중요합니다. 일반 Java POJO에서는 값을 가져올 수 없습니다.

또한, Spring은 static 변수에는 값을 주입할 수 없습니다. [참고자료](https://stackoverflow.com/questions/45192373/how-to-assign-a-value-from-application-properties-to-a-static-variable)

Spring 3.x 버전에서는 *.properties 파일에 주입하려고 했던 변수명과 값이 없으면, null로 주입하였습니다. 하지만, Spring Boot 에서는 PropertySourcesPlaceholderConfigurer 가 기본적으로 Bean 설정되어있습니다. PropertySourcesPlaceholderConfigurer는 Property 값을 설정하도록 강제합니다. [참고자료](https://stackoverflow.com/questions/11991194/can-i-set-null-as-the-default-value-for-a-value-in-spring)

1. *.properties 이름과 값이 없을 때: Bean 생성 시, 오류를 발생시켜 Spring Boot가 실행되지 않도록 합니다.
2. `@Value("${var.value:null}")` 처럼 @Value 어노테이션 안에서 `:` 문자 뒤에 기본 설정 값이 들어가면, 해당 값으로 주입합니다. 여기서, null을 문자열 `"null"`이 아닌 실제 `null` 값으로 설정하려면 SpEL 문법을 사용하여 `@Value("${var.value:#{null}}")` 형태로 사용하면 됩니다.

<br>

### 참고자료

https://www.baeldung.com/spring-value-annotation

https://www.baeldung.com/spring-value-defaults

https://stackoverflow.com/questions/43778290/how-to-use-value-on-spring-non-managed-class

https://stackoverflow.com/questions/45192373/how-to-assign-a-value-from-application-properties-to-a-static-variable

https://stackoverflow.com/questions/11991194/can-i-set-null-as-the-default-value-for-a-value-in-spring