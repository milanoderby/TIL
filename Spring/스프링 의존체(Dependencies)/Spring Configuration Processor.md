# Spring Boot Configuration Metadata

### Configuration Properties

 Spring Application을 개발할 때, 빈(Beans)을 생성하거나 의존객체를 가져옵니다. Spring을 통해 Bean을 생성할 때, 초기 설정 값(Configuration Properties)을 부여할 수 있습니다. 일반적으로, 설정 값(Configuration Properties)은 `.properties` 파일 또는 `.yaml` 파일에 작성하며, Spring Application이 빌드될 때, 이를 읽어와 Spring Bean에 주입하는 방식을 이용합니다.

<br>

### Configuration Metadata 도입 배경

 그러나, 개발자가 직접 생성한 빈이 아닌 Maven 이나 Gradle로 가져온 의존객체인 경우, 의존객체에 대해서 어떤 값들을 설정할 수 있고, 기본 설정값(default Value)인지, 어떤 것인지 알기가 어렵습니다. 심지어 이전에 공식문서를 통해 어떤 설정 값이 있는지 알았지만, 버전에 따라 deprecated(더 이상 사용되지 않는)된 property도 있는데, 이를 파악하기 어렵습니다.

 이러한 어려움을 해결하기 위해 Spring Boot에서는 ***Configuration Meatadata***라는 JSON 파일을 만들었습니다. 이 파일은 설정 값들(Configuration Properties) 에 대한 필수적인 정보를 서술한 파일입니다. 이 파일은 IDE(IntelliJ Ultimate)가 인식가능하여 자동완성(autocomplete)과 설정할 때, 힌트(Configuration Hints)를 제공하도록 도움을 줍니다.

<br>

### Spring Configuration Processor

 Spring Boot에서는 여러분이 직접 만든 Bean의 설정가능한 값에 대한 정보를 알려주는 ***Configuration Metadata*** 파일을 쉽게 생성하는 방법을 제공합니다. `spring-boot-configuration-processor` 의존성을 추가하고, `@ConfigurationProperties` 애노테이션을 Bean에 붙이면, 프로젝트가 빌드(컴파일)될 때, Java annotation processor을 호출하여 ***Configuration Metadata*** 파일을 생성합니다.

<br>

### @Configuration Properties

실제 사용방법은 아래와 같습니다.

설정 값을 저장할 Java Bean에 `@ConfigurationProperties` 애노테이션을 붙입니다.

```java
@Configuration
@ConfigurationProperties(prefix = "database")
public class DatabaseProperties {
	
    public static class Server {

        private String ip;
        private int port;

        // standard getters and setters
    }
	
    private String username;
    private String password;
    private Server server;
	
    // standard getters and setters
}
```

<br>

`~.properties` 파일을 생성하고, 아래와 같이 작성합니다.

```yaml
# Test DB Properties
database.username=baeldung
database.password=password
database.server.ip=223.123.123.123
database.server.port=3306
```

<br>

Spring Application을 빌드하면, `.properties` 파일에 설정한 초기 설정 값들이 Java Bean의 필드 값으로 할당됩니다. 또한, `spring-configuration-metada.json` 파일이 `target/classes/META-INF` 디렉토리에 생성됩니다. (maven structure 기준)

### Cofiguration Metadata

위에서 생성된 `spring-configuration-metada.json` 파일은 아래와 같습니다.

```json
{
  "groups": [
    {
      "name": "database",
      "type": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties"
    },
    {
      "name": "database.server",
      "type": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties$Server",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties",
      "sourceMethod": "getServer()"
    }
  ],
  "properties": [
    {
      "name": "database.password",
      "type": "java.lang.String",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties"
    },
    {
      "name": "database.server.ip",
      "type": "java.lang.String",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties$Server"
    },
    {
      "name": "database.server.port",
      "type": "java.lang.Integer",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties$Server",
      "defaultValue": 0
    },
    {
      "name": "database.username",
      "type": "java.lang.String",
      "sourceType": "com.baeldung.autoconfiguration.annotationprocessor.DatabaseProperties"
    }
  ],
  "hints": []
}
```

<br>

Java Bean의 필드에 JavaDoc comment를 더하면, JSON 파일의 해당 프로퍼티에 `description` 항목으로 추가됩니다.

또한, 필드에 @Min, @Max 애노테이션 등 hibernate Bean Validation 애노테이션을 붙이면, `.properties` 파일에서 해당 프로퍼티 값을 작성할 때, IDE에서 프로퍼티가 가질 수 있는 값에 대한 설정 힌트(Configuration Hint)를 제공합니다.

<br>

### 참고자료

https://medium.com/the-mighty-programmer/spring-boot-two-ways-documentation-7926dd059d14

https://www.baeldung.com/spring-boot-configuration-metadata

https://docs.spring.io/spring-boot/docs/current/reference/html/configuration-metadata.html#configuration-metadata