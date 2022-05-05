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

데이터를 저장할 DB 설정을 의미합니다. h2 console을 이용하여 DB에 직접 접근할 때, 위 설정 값을 사용합니다.

url의 `mydb` 값은 database의 이름을 지정하는 것이므로 다른 값을 사용해도 무방합니다.

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
    database-platform: org.hibernate.dialect.H2Dialect
    show-sql: true
    generate-ddl: false
    hibernate:
      ddl-auto: none
```

JPA는 코드 단에서 Data에 접근할 때, 사용하는 기술 중 하나입니다.

위에서 사용된 프로퍼티는 각각 아래와 같습니다.

spring.jpa.database-platform: 각 DB 별로 문법이 조금씩 다르기 때문에 해당 문법에 맞춰서 내부적으로 SQL을 변형하기 위해 사용하는 DB 플랫폼을 지정

spring.jpa.show-sql: JPA 가 실행하는 SQL을 콘솔창을 통해 보여줄지 여부를 지정 (개발환경에서만 사용하고, 운영환경에서는 사용하지 않습니다.)

spring.jpa.generate-ddl: JPA는 DDL 생성하는 기능이 있습니다. 이 기능을 ON/OFF 할 지를 결정하는 옵션이며, 사용하는 DB에 독립적으로 동작합니다. 이 설정만으로는 DDL 생성기능을 완전히 제어할 수 없습니다. 왜냐하면, JPA의 실제 구현체인 hibernate에도 DDL 생성기능이 있기 때문입니다. 그렇기 때문에 아래 설정을 추가로 확인해야 됩니다.

spring.jpa.hibernate.ddl-auto: JPA 구현체인 Hibernate의 기능이며, DDL 생성을 더 상세하게 설정하는 값 입니다. schema manager?(schema를 관리하는 SQL문을 의미하는 것으로 보입니다.)가 없다면, `create-drop`으로 설정됩니다. 그 외에는 모두 `none`으로 설정됩니다.

자세한 설명은 [Spring Boot의 DB 초기화](https://github.com/milanoderby/TIL/blob/master/Spring/Spring%20Boot%EC%9D%98%20DB%EC%B4%88%EA%B8%B0%ED%99%94.md)를 참고하세요.

<br>

### Spring Boot에서 지원하는 SQL 스크리브를 이용한 DB 초기화

스프링 부트에서는 classpath root 경로(resources 디렉토리)에 있는`schema.sql` 과 `data.sql` 파일에 있는 DDL 스크립트 및 DML 스크립트를 이용하여 Datasource로 사용하는 DB를 초기화할 수 있습니다.

SQL DB 초기화는 기본적으로 내장 인메모리 DB(hsqldb, h2, derby 등)를 사용할 때에만 실행됩니다.

자세한 설명은 [Spring Boot의 DB 초기화](https://github.com/milanoderby/TIL/blob/master/Spring/Spring%20Boot%EC%9D%98%20DB%EC%B4%88%EA%B8%B0%ED%99%94.md)를 참고하세요.

#### schema.sql

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

#### data.sql

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

