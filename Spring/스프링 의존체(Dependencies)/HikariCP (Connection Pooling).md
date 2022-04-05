# HikariCP (Connection Pool)

Hikari CP는 Spring Data에서 가장 선호하는 Connection Pool 구현체입니다.

spring-jdbc, spring-data-jdbc, spring-data-jpa 등 의존성을 추가하면, 내부적으로 의존성을 포함하고 있어 자동으로 의존성이 추가됩니다.

<br>

### Connection Pool 이 필요한 이유

먼저, 내부적으로 Connection Pooling 기술을 사용하는 Datasource 구현체를 사용해야하는 필요성을 알아보겠습니다. 

1. DB Connection 객체 생성 시, 소요되는 비용 (메모리 할당 및 소요시간) 은 상당합니다.

2. 이 말은, 우리가 어떤 쿼리를 수행할 때 마다, DB Connection 객체를 생성한다면, Application Server에는 큰 부담이 가중될 것 이라는 의미입니다.

3. 그래서, 우리는 매번 DB Connection 을 생성 및 반환/폐기하는 방식이 아닌 미리 Connection Pool을 생성해두고, 쿼리를 수행할 때마다, 객체를 반환하는 방식을 사용합니다. 단순히, 객체가 반환되어 Connection Pool에 보관되는 것일 뿐, 객체가 파기되는 것이 아님에 주의합니다.

<br>

### Hikari Datasource 설정 과정

1. HikariConfig 에 설정 값을 입력
2. HikariConfig 을 인자로 받아 HikariDatasource 생성 (Datasource 구현체로 HilkariDatasource를 사용하는 것입니다. 다른 구현체를 사용한다면, 이 부분을 수정하면 됩니다.)
3. HikariDatasource 에서 getConnection() 메소드로 Connection 객체를 가져옵니다.
4. Connection 객체를 이용하여 SQL문 실행

여기서, 1 ~ 2번 과정까지가 HikdariCP 설정과 관련된 부분입니다.

여기서, 3 ~ 4번 과정은 spring-jdbc, spring-data-jdbc, spring-data-jpa 중 어떤 기술을 사용하느냐에 따라 실제 구현이 달라질 수 있습니다.

<br>

### Hikari CP 주요 설정

#### 1. 필수 설정

##### datasourceClassName

> JDBC 드라이버에 의해 제공되는 DataSource 클래스명입니다. 
>
> 각각의 JDBC 드라이버에서 제공하는 Datasource 클래스명을 파악하려면, JDBC 드라이버의 스펙을 확인하거나, 다음 [표](https://github.com/brettwooldridge/HikariCP#popular-datasource-class-names)를 참고하세요. 
>
> 만약, "오래된 방식인" DriverManager 기반 JDBC 드라이버 설정을 위해 `jdbcUrl` 값을 설정하고 있다면, `datasourceClassName` 값은 설정하지 않아도 됩니다.
>
> 기본 값: None

**또는**

##### jdbcUrl

> 이 값은 HicariCP 에게 "DriverManager" 기반 설정을 사용하도록 알려줍니다. 
>
> "DataSource " 기반 설정이 여러 방면에서 더 우수하다고 생각할 수 있지만, 그러나, 많은 결과물(영어로는 deployments)에서 유의미한 차이가 거의 없습니다.
>
> `jdbcUrl` 설정을 "오래된" 드라이버와 사용한다면, `driverClassName` 값도 설정해야할 것입니다. 하지만, 우선 `driverClassName` 값을 설정하지 말고, 시도해보세요.
>
> `jdbcUrl` 설정 값을 사용하면, 여전히 DataSource 프로퍼티를 사용하여 driver를 설정할 수 있습니다. 그리고, 이 방식이 URL 자체에 명시된 driver 인자를 사용하는 것보다 권장하는 방식입니다.
>
> 기본 값: None

<br>

##### username

> 이 값은 인증 사용자명 JDBC Driver 에서 Connection을 가져올 때, 사용되는 인증 사용자이름을 설정하는 값입니다.
>
> DataSource 설정으로 동작할 때는, `DataSource.getConnection(username, password)` 를 호출하는 결정적인 방식으로 사용됩니다.
>
> 그러나, Driver 기반 설정에서는, 모든 드라이버가 다릅니다. Driver 기반의 경우, HikariCP는 `username` 을 이용하여 `Properties` 내의 `user` 값을 설정하고, 이 값을 `DriverManager.getConnection(jdbcUrl, props)` 메소드 호출의 인자로 전달합니다. 만약, 이 방식을 원하지 않는다면, 위 메소드 호출을 건너뛰고, `addDataSourceProperty("username", ...)`를 호출하세요.
>
> 기본 값: None

<br>

##### password

> 이 값은 인증 사용자명 JDBC Driver 에서 Connection을 가져올 때, 사용되는 인증 사용자이름을 설정하는 값입니다.
>
> DataSource 설정으로 동작할 때는, `DataSource.getConnection(username, password)` 를 호출하는 결정적인 방식으로 사용됩니다.
>
> 그러나, Driver 기반 설정에서는, 모든 드라이버가 다릅니다. Driver 기반의 경우, HikariCP는 `password` 을 이용하여 `Properties` 내의 `password` 값을 설정하고, 이 값을 `DriverManager.getConnection(jdbcUrl, props)` 메소드 호출의 인자로 전달합니다. 만약, 이 방식을 원하지 않는다면, 위 메소드 호출을 건너뛰고, `addDataSourceProperty("pass", ...)`를 호출하세요.
>
> 기본 값: None

<br>

#### 2. 자주 사용되는 설정

##### autoCommit

> 이 값은 Pool에서 가져오는 Connection이 기본적으로 auto-commit 할지를 설정하는 값입니다.
>
> 기본 값: true

<br>

##### connectionTimeout 

> 클라이언트가 Pool에서 Connection을 가져오는데 기다리는 최대시간(단위: milliseconds)을 설정합니다.
>
> 설정한 시간을 초과하면 SQLException이 발생합니다.
>
> 허용 가능한 최솟값은 250ms 입니다.
>
> 기본 값: 30000 (30초)

 <br>

##### idleTimeout

> Connection이 Pool에서 유휴(idle) 상태로 존재할 수 있는 최대 시간을 설정합니다.
>
> 이 설정은 `minimumIdle`이 `maximumPoolSize` 보다 작은 경우에만 동작합니다.
>
> pool에 있는 connection의 개수가 `minimumIdle`보다 클 경우, idle connection은 `idleTimeout` 만큼 시간이 지나면, 파기됩니다.
>
> pool에 있는 connection의 개수가 `minimumIdle` 에 도달할 경우, idle connection은 더 이상 파기되지 않습니다.
>
> 허용 가능한 최솟값은 10000ms(10초) 입니다.
>
> 기본 값: 600000 (10분)

<br>

##### keepaliveTime

> 이 값은 HikariCP가 Connection이 database 또는 network 인프라에 의해 타임아웃되지 않고, 살아있도록 유지하기 위해 ping 테스트를 하는 빈도를 조정하는 값입니다.
>
> 해당 값은 `maxLifetime` 값보다 반드시 작아야합니다.
>
> "keepalive" 행위는 idle connection에서만 발생합니다. 
>
> 주어진 connection에 대해 `keepalliveTime` 에 도달하면, pool에서 잠시 제거 후, "ping" 을 수행한 다음, pool에 다시 넣습니다.
>
> 여기서, "ping" 행위는 **JDBC4의 isValid() 메소드 호출** 또는 **connectionTestQuery 실행**을 의미합니다.
>
> 일반적으로, "ping" 행위를 위해 pool 바깥에 잠시 나와있는 시간은 한자릿수의 milliseconds 정도로 측정되기 때문에, 엄청나게 성능에 안좋은 영향을 끼치지는 않습니다.
>
> 허용 가능한 최솟값은 30000ms(30초) 입니다.
>
> 권장하는 값은 분 단위입니다.
>
> 기본 값: 0 (disabled)

<br>

##### maxLifeTime

> connection의 최대 생명 기간을 설정합니다.
>
> 현재, 사용 중인 connection은 절대 파기되지 않으며, 사용이 끝나고나서 제거됩니다.
>
> 하지만 유휴 커넥션은 바로 폐기됩니다.
>
> **이 값을 database infrastructure에 설정된 connection time limit 값보다 2~3초 정도 짧게 설정하는 것을 권장합니다.**
>
> 기본 값: 1800000 (30분)

<br>

##### connectionTestQuery

> **당신이 사용하는 드라이버가 JDBC4를 지원하는 경우 이 속성을 설정하지 않는 것을 추천합니다.**
>
> 이 속성은 JDBC4의 Connection.isValid() API를 지원하지 않는 "legacy" 드라이버를 위한 속성입니다.
>
> 이 속성의 값은 pool에서 connection을 꺼내온 직후, 해당 connection이 database와의 연결상태가 유효한지 검증하기위해 실행되는 쿼리입니다.
>
> **다시 한번 말하지만, 우선, 이 속성 값을 설정하지 말고, 실행하세요.**
>
> 만약, 당신이 사용하는 드라이버가 JDBC4를 지원하지 않을 경우, HikariCP가 error log를 남길 것입니다.
>
> 기본 값: none

<br>

##### minimumIdle

> 이 속성은 connection pool에서 유지할 최소 idle connection 개수를 설정합니다.
>
> idle connection의 개수가 이 속성 값보다 작고, 전체 connection의 개수가 `maximumPoolSize` 보다 작아질 경우, HikariCP는 빠르고, 효율적으로 추가 connection을 생성하기 위해 노력할 것입니다. **그러나, 급증하는 요청에 대해 최대 성능과 응답성을 원한다면, 이 값을 설정하지 않는 것을 권장합니다.**
>
> 대신, HikariCP가 고정된 크기의 connection pool처럼 작동하도록 하는 것을 권장합니다.
>
> 기본 값: maximumPoolSize값과 동일

<br>

##### maximumPoolSize

> 유휴(idle)상태, 사용 중인 connection을 포함하여 pool에서 보관가능한 최대 커넥션 개수를 설정합니다.
>
> 일반적으로, 이 속성 값은 DB 백엔드와 연결된 실제 connection의 최대 개수를 명시합니다.
>
> 이 속성 값에 대해 합리적인 값은 당신의 실행환경에 의해 결정됩니다.
>
> pool의 connection 개수가 `maximumPoolSize` 값에 도달하면, 사용가능한 idle connection이 없어지고, getConnection() 메소드 호출은 block 될 것 입니다. 그 후, `connectionTimeout` milliseconds 만큼 대기 후, timeout되어 SQLException이 발생할 것입니다. 자세한 사항은 [Pool Sizing에 관하여](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)를 읽어보세요.
>
> 기본 값: 10

<br>

##### metricRegistry

> 이 속성은 프로그래밍적으로 설정하거나, IoC 컨테이너에 의해 사용가능한 설정입니다. 이 속성은 *Codahale/Dropwizard* `MetricRegistry` 인스턴스를 명시할 수 있습니다. 이 인스턴스는 다양한 측정치(metrics)를 기록하기 위해 사용됩니다. 자세한 측정 값은 [Metrics](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-Metrics)를 참고하세요.
>
> 기본 값: none

<br>

##### healthCheckRegistry

> 이 속성은 프로그래밍적으로 설정하거나, IoC 컨테이너에 의해 사용가능한 설정입니다. 이 속성은 *Codahale/Dropwizard* `HealthCheckRegistry` 인스턴스를 명시할 수 있습니다. 이 인스턴스는 현재의 health 정보를 보고하기 위해 사용됩니다. 자세한 측정 값은 [Health Checks](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-HealthChecks)를 참고하세요.
>
> 기본 값: none

<br>

##### poolName

> 이 속성은 사용자가 정의한 connection pool의 이름을 의미합니다. 주로, logging과 JMX관리 콘솔 창에서 나타납니다. 그로 인해 각각의 pool과 pool의 설정을 인식할 수 있습니다.
>
> 기본 값: auto-generated

<br>

#### 3. 자주 사용되지 않는 설정

이 설정들을 한번씩 살펴본 결과, 굳이 한글로 정리할 필요가 없다고, 느꼈습니다.

상세한 설정은 [자주 사용되지 않는 설정](https://github.com/brettwooldridge/HikariCP#infrequently-used)을 참고하세요.

<br>

### Hikari CP 최적 설정 값

| 속성 명             | 속성 의미                                              | 예시 데이터 및 최적 설정                                     |
| ------------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| jdbcUrl             | DB URL                                                 | jdbc:mysql://localhost:3306/simpsons                         |
| username            | DB 사용자명                                            | test                                                         |
| password            | DB 패스워드                                            | test                                                         |
| autoCommit          | 자동 commit 여부                                       | 기본 값이 true이기 때문에, 굳이 설정하지 않아도 됩니다.      |
| connectionTimeout   | pool에서 가져오는데 걸리는 timeout                     | 2000 ~ 3000 (2~3초)                                          |
| idleTimeout         | idle connection 상태로 pool에서 남을 수 있는 시간 제한 | 설정하지 않는 것을 권장                                      |
| keepaliveTime       | idle connection 이 유효한지 test하는 주기              | 600000 (10분)                                                |
| maxLifeTime         | connection의 최대 생명 기간                            | database infrastructure에 설정된 connection time limit 값보다 2~3초 정도 짧게 설정 |
| connectionTestQuery | idle connection이 유효한 연결인지 확인하는 쿼리문      | 설정하지 않는 것을 권장 (실행했을 때, driver가 JDBC4를 지원하지 않을 때에만 설정) |
| minimumIdle         | pool에서 최소로 유지할 idle connection의 개수          | 설정하지 않는 것을 권장                                      |
| maximumPoolSize     | pool내의 connection의 최대 갯수                        | [Pool Sizing에 관하여](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing) 참고 후, 테스트를 통해 설정할 것을 권장 |
| metricRegistry      | 여러 측정값을 확인하고 싶을 때, 설정                   |                                                              |
| healthCheckRegistry | hikariCP의 상태를 확인하고 싶을 때, 설정               |                                                              |
| poolName            | 사용자가 여러 개의 pool을 구분하기 위한 값             | mySqlPool                                                    |

<br>

### 참고자료

https://github.com/brettwooldridge/HikariCP#configuration-knobs-baby