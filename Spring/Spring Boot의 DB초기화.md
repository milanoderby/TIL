# Spring Boot의 DB초기화

Spring Boot 기동 시, DB의 schema 생성 및 생성된 테이블에 데이터를 추가하는 2가지 방법을 설명할 것입니다.

1. JPA 구현체인 Hibernate에서 지원하는 DB 초기화 방식
2. Spring Boot에서 지원한는 SQL 스크립트를 이용한 DB 초기화 방식

일반적으로 2번 방식을 고려할 수 있으나, Data Access 할 때, JPA를 사용하고 JPA 구현체로 Hibernate를 사용한다면, 1번 방식을 사용할 수도 있습니다.

<br>

### 1. JPA 구현체인 Hibernate에서 지원하는 DB 초기화 방식

#### 관련 설정 값

spring.jpa.generate-ddl: JPA는 DDL 생성하는 기능이 있습니다. 이 기능을 ON/OFF 할 지를 결정하는 옵션이며, 사용하는 DB에 독립적으로 동작합니다. 이 설정만으로는 DDL 생성기능을 완전히 제어할 수 없습니다. 왜냐하면, JPA의 실제 구현체인 hibernate에도 DDL 생성기능이 있기 때문입니다. 그렇기 때문에 아래 설정을 추가로 확인해야 됩니다.

spring.jpa.hibernate.ddl-auto: JPA 구현체인 Hibernate의 기능이며, DDL 생성을 더 상세하게 설정하는 값 입니다. schema manager?(schema를 관리하는 SQL문을 의미하는 것으로 보입니다.)가 없다면, `create-drop`으로 설정됩니다. 그 외에는 모두 `none`으로 설정됩니다. 기본 설정 값이 애매하니 가급적 설정 값을 제대로 명시해주세요.

가능한 값은 아래와 같습니다.

- none: DDL 생성기능을 사용하지 않습니다.
- create: 시작시, 기존에 존재하는 테이블을 삭제하고, JPA에서 사용하는 객체 모델에 맞게 새로운 테이블을 생성합니다.
- create-drop: create 설정이 하는 기능에 추가로 종료 시에도 테이블을 삭제합니다.
- update: @Entity 어노테이션이 붙은 객체 모델 또는 XML로 생성한 객체 모델이 DB에 존재하는 스키마와 맞지 않을 경우, 객체모델에 맞게 스키마를 변경합니다. 어플리케이션에서 사용되지 않는 테이블 또는 컬럼일지라도 절대 삭제하지는 않습니다.
- validate: JPA에서 사용되는 테이블이나 컬럼이 존재하는지 검증만 합니다. 만약 없다면, 예외를 던집니다.

`import.sql` 파일이 classpath root 경로(resources 디렉토리)에 있고, 설정 값이 `create` 또는 `create-drop`이면, hibernate가 import.sql 에 명시된 쿼리를 수행할 것 입니다. (이는 Spring 과 별개인 Hibernate 기능입니다.)

<br>

### 2. Spring Boot에서 지원하는 SQL 스크립트를 이용한 DB 초기화 방식

스프링 부트에서는 classpath root 경로(resources 디렉토리)에 있는`schema.sql` 과 `data.sql` 파일에 있는 DDL 스크립트 및 DML 스크립트를 이용하여 Datasource로 사용하는 DB를 초기화할 수 있습니다.

SQL DB 초기화는 기본적으로 내장 인메모리 DB(hsqldb, h2, derby 등)를 사용할 때에만 실행됩니다.

#### 관련 설정 값

spring.sql.init.platform: schema.sql, data.sql 중 특정 SQL문만 실행하고 싶을 때, 설정하는 값입니다. 예를 들어 `someplatform` 이라는 값으로 설정하면, `schema-someplatform.sql`, `data-someplatform.sql` 파일만 실행됩니다. 기본 값은 `all` 이기 때문에 모든 `schema~.sql` 파일과 `data~.sql`파일이 실행됩니다.

spring.sql.init.mode: SQL 을 이용한 DB 초기화 여부를 결정하는 조건 값입니다. 기본 값은 `embedded` 입니다.

가능한 값은 아래와 같습니다.

- embedded: datasource로 사용하는 DB가 내장 DB 즉, 인메모리 DB일 때에만 DB 초기화를 수행합니다.
- always: DB 종류에 상관 없이 항상 DB 초기화를 수행합니다.
- never: DB 종류에 상관 없이 항상 DB 초기화를 수행하지 않습니다.

spring.sql.init.continue-on-error: SQL 스크립트로 DB 초기화 중 예외 발생 시, 어플리케이션을 구동할지 말지에 대한 설정 값입니다. 스프링 부트는 기본적으로 SQL 스크립트로 DB 초기화 수행 시, fail-fast 기능을 사용합니다. 즉, SQL 스크립트를 수행하다가 예외가 발생하면, 어플리케이션은 시작하지 않습니다. 기본 값은 `false` 입니다.

가능한 값은 아래와 같습니다.

- true: SQL 스크립트 수행 중 예외 발생해도 어플리케이션 시작
- false: SQL 스크립트 수행 중 예외 발생하면, 어플리케이션 중지

<br>

### Hibernate 에서 지원하는 DB 초기화와 Spring Boot에서 지원하는 SQL 스크립트를 이용한 DB 초기화의 실행순서

기본적으로 DB를 초기화하는 2가지 방식을 모두 사용하는 것은 권장하지 않습니다. 

하지만, 필요에 의해 2가지 방식을 모두 사용하게 된다면 어떤 방식이 먼저 실행될까요?

스프링 부트에서 지원하는 **SQL 스크립트를 이용한 DB 초기화**가 **Hibernate에서 지원하는 DB초기화** 보다 먼저 발생합니다.

하지만, spring.jpa.defer-datasource-initialization 설정 값을 `true`로 설정하면, **Hibernate 에서 지원하는 DB 초기화** 가 실행된 후, **SQL 스크립트를 이용한 DB 초기화**가 발생합니다.

<br>

### 참고자료

https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto.data-initialization.using-jpa