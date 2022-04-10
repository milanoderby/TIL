# Tomcat 로깅 정책 설정 - logback

Tomcat Log의 경우, 팀에서는 ```logback-spring.xml``` 을 통해서 로깅(file-logging, lnc-logging) 정책을 관리하고 있습니다.
아래는 코드를 일부 발췌하여 어떻게 관리되는지 확인해보겠습니다.

* `FILE_MAX_HISTORY` 변수의 값을 설정: 최대 14일의 로그만 보관하도록 설정한 예시입니다.

``` xml
<springProperty name="FILE_MAX_HISTORY" source="logging.file.max-history" defaultValue="14"/>
```
<br>

* `LOG_FILE` 변수의 값을 설정: 로그파일 명을 설정한 예시입니다.

``` xml
<property name="LOG_FILE" value="${LOG_FILE:-${LOG_PATH:-${LOG_TEMP:-${java.io.tmpdir:-/tmp}}/}spring.log}"/>
```
<br>

* `file-logging` 설정

``` xml
<springProfile name="file-logging">
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
      <encoder>
        <pattern>${FILE_LOG_PATTERN}</pattern>
      </encoder>
      <file>${LOG_FILE}</file>
      <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}</fileNamePattern>
        <maxHistory>${FILE_MAX_HISTORY}</maxHistory>
      </rollingPolicy>
    </appender>

    <appender name="FILE-ASYNC" class="ch.qos.logback.classic.AsyncAppender">
      <appender-ref ref="FILE"/>
    </appender>
  </springProfile>
```

```rollingPolicy``` 영역에서 로깅 정책을 수립합니다.

* ````fileNamePattern````: 로그가 쌓이는 파일에서 이동할 파일명을 명시합니다.
* ````maxHistory````: 로그를 보관할 최대일수를 명시합니다.

rollingPolicy 영역의 설정을 통해서 관리를 하고 있습니다.
