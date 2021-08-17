# Java Functional Interface

Java 1.8(이하, Java 8) 부터 도입된 굉장히 중요한 Java language 스펙입니다.

Java 8부터 도입된 함수형 인터페이스는 java.util.function 패키지에 정의되어 있습니다.

<br>

### 도입배경

 Java 8 부터는 람다표현식 형태의 문법적인 향상을 추구하였습니다. 람다표현식은 메소드로 전달될 수도 있고, 메소드에서 반환할 수도 있습니다.

 Java 8 이전에는 한가지 기능만 수행하는 메소드를 캡슐화하기 위해 클래스를 생성해야했습니다. 즉, 하나의 함수 표현식을 정의하기 위해 불필요한 관례적인(boilerplate) 코드들을 작성해줘야했습니다.

이러한 불편한 사항을 

<br>

### 개념



<br>

### @FunctionalInterface

**Java 8에서 추가된 함수형 인터페이스**에는 모두 FunctionalInterface 애노테이션이 붙어있습니다. 

하지만, 이 애노테이션을 붙이지 않아도 컴파일러는 **함수형 인터페이스의 스펙을 지키는 커스텀 인터페이스**도 함수형 인터페이스로 인식가능하며, 함수형 인터페이스처럼 다룰 수 있습니다. 그렇다면, 왜 붙이는 것일까요?

컴파일러가 **애노테이션이 붙은 인터페이스가 함수형 인터페이스의 디자인 의도를 위반하는 인터페이스인지 아닌지를** 파악할 수 있도록 도움을 주기위함입니다. 이 애노테이션이 붙어있을 때, 다음 2가지 사항을 위반하면, 에러메세지를 출력합니다.

1. 애노테이션이 붙은 타입이 Interface 타입이어야합니다.
2. 애노테이션이 붙은 함수형 인터페이스의 필수조건을 충족해야합니다.

<br>

### 기존 Functional Interface

**Java 8 이전 버전부터 존재하던 인터페이스** 중에서도 함수형 인터페이스의 제약조건을 충족하는 인터페이스들이 있습니다. Java 8에서는 이러한 인터페이스들에 

<br>

### 참고자료

https://www.baeldung.com/java-8-functional-interfaces

https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html#package.description