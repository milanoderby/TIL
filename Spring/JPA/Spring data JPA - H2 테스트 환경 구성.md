# Spring data JPA - H2 테스트 환경 구성

### 프로젝트 설정

IntelliJ > New > Project > Spring Initializr 에서

프로젝트 빌드 툴: Gradle

언어: Kotlin

Java 버전: 11

패키지 종류: Jar

아티팩트 명: 프로젝트 특성에 맞게 설정

<br>

### Spring Boot 의존성 추가

Spring Web

Spring Data JPA

H2 Database

선택 후, 종료합니다.

<br>

### datasource 설정

간단하게 spring boot auto configure 를 이용한 설정으로 진행합니다.

application.yaml 파일을 아래와 같이 추가합니다.

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:mydb
    username: sa
    password: password
    driverClassName: org.h2.Driver
```

위는 데이터를 가져올 DB 설정을 의미합니다.

<br>

### H2 DB 설정

```yaml
spring:
  datasource:
  ...

  h2:
    console:
      enabled: true
      path: /h2-console
      settings:
        web-allow-others: false
```

H2 DB는 인메모리 DB이기 때문에 MySQL 워크벤치와 같은 Console 기능을 웹 서비스를 통해 제공합니다.

위에서 사용된 프로퍼티는 각각 아래와 같습니다.

spring.h2.console.enabled: H2 Console 기능 사용가능 여부

spring.h2.console.path: H2 Console 기능 서비스 경로 지정

spring.h2.console.settings.web-allow-others: H2 Console 외부 통신 허용여부

<br>

### JPA 설정

```yaml
spring:
  datasource:
  ...
  
  h2:
  ...
  
  jpa:
    spring.jpa.database-platform: org.hibernate.dialect.H2Dialect
    show-sql: true
```

JPA는 코드 단에서 Data에 접근할 때, 사용하는 기술 중 하나입니다.

위에서 사용된 프로퍼티는 각각 아래와 같습니다.

spring.jpa.spring.jpa.database-platform: 각 DB 별로 문법이 조금씩 다르기 때문에 해당 문법에 맞춰서 내부적으로 SQL을 변형하기 위해 사용하는 DB 플랫폼을 지정

spring.jpa.show-sql: JPA 가 실행하는 SQL을 보여줄지 여부를 지정

<br>

### schema.sql

```sql
CREATE TABLE COUNTRIES (
	id VARCHAR(20) ,
	name VARCHAR(20),
	
	primary key (id)
);

```

H2 는 인메모리 DB이기 때문에 처음에 어떤 테이블도 없습니다.

schema.sql 을 통하여 테이블 스키마를 초기화합니다.

<br>

### data.sql

```sql
INSERT INTO countries (id, name) VALUES (1, 'USA');
INSERT INTO countries (id, name) VALUES (2, 'France');
INSERT INTO countries (id, name) VALUES (3, 'Brazil');
INSERT INTO countries (id, name) VALUES (4, 'Italy');
INSERT INTO countries (id, name) VALUES (5, 'Canada');
```

H2 는 인메모리 DB이기 때문에 테이블 내에 어떤 데이터도 없습니다.

data.sql 을 통하여 테이블 내에 데이터를 추가합니다.

<br>

### H2 DB 접속 (콘솔 창 이용)

위에서 명시한 경로를 이용하여 H2 콘솔 창에 접속합니다.

http://localhost:8080/h2-console

서버포트는 별도 설정을 하지 않았다면, 8080일 것입니다.

application.yml 파일의 정보를 이용하여 url, id, password를 입력 후, 접속합니다.

<br>

### 참고자료

https://www.baeldung.com/spring-boot-h2-database