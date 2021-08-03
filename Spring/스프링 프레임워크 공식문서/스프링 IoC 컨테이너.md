# 스프링 IoC 컨테이너

이 글은 스프링 프레임워크 공식문서를 참고하여 정리한 문서입니다.

참고: https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core

## 1. 스프링 IoC 컨테이너와 Bean 소개

`IoC 컨테이너`는 의존성 주입(DI)으로도 알려져있습니다. 

DI란 `어떤 객체`가 본인이 사용할 `의존객체`를 오로지 다음 방식으로만 정의하는 것을 말합니다.

1. 생성자 인자로 전달받아 저장
2. 팩토리 메소드 인자로 전달받아 저장
3. 팩토리 메소드로 생성된 객체의 프로퍼티를 세팅

<br>

`스프링 IoC 컨테이너`는 객체를 생성할 때, 해당 객체가 사용하는 의존객체들을 주입해주는 역할을 합니다.

이 과정은 근본적으로 `Bean`이 스스로 `의존객체의 생성을 통제하는 개념`이 아니고, 반대로 `의존객체를 주입받는 상황`이기 때문에 `제어의 역전`(IoC) 라고 부릅니다.

<br>

`Bean Factory` 인터페이스는 객체관리를 위한 더나은 설정메커니즘을 제공합니다.

`ApplicationContext` 는 `BeanFactory`의 하위 인터페이스이며, 아래의 기능들이 추가되었습니다.

- Spring AOP 기능과의 통합이 쉬워짐
- Message Resource 처리(국제화 사용을 위해)
- Event 발행
- `WebApplicationContext` 같은 어플리케이션 계층에 특화된 컨텍스트들(웹 어플리케이션 사용을 위해)

요약하자면, `BeanFactory`는 프레임워크 설정과 기본적인 기능을 제공하며, `ApplicationContext` 는 좀더 엔터프라이즈에 특화된 기능들을 제공합니다.

<br>

스프링 IoC 컨테이너에 의해 관리되는 객체들을 `Bean` 이라고 부르며, Bean은 스프링 IoC 컨테이너에 의해 인스턴스화, 조립, 관리됩니다. Bean과 Bean들간의 의존성은 구성 메타데이터(configuration metadata)에 저장되며, `IoC 컨테이너`는 이러한 구성 메타데이터를 이용합니다.

<br>

## 2. 스프링 IoC 컨테이너 개요

