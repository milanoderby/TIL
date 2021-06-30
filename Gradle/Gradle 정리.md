# Gradle Plugin을 이용하여 Spring Boot 개발하기

참고자료: https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/reference/htmlsingle/

## 1. 소개

 스프링부트 그레이들 플러그인은 스프링부트를 지원을 제공합니다. 이것은 여러분에게 다음을 가능하게 합니다.

- `실행가능한 jar` 또는 `war` 로 패키징할 수 있게 합니다.
- 스프링부트 어플리케이션을 실행가능하게 합니다.
- ` 스프링부트의 의존성들`의 의존성관리를 제공합니다.

스프링부트 그레이들 플러그인은 그레이들 6.8, 6.9, 7.x 버전을 요구하며, 그레이들의 [설정 캐시](https://docs.gradle.org/current/userguide/configuration_cache.html) 와 함께 사용될 수도 있습니다. 사용자 가이드 뿐만 아니라 [API 문서](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/api) 도 참고가능합니다.

<br>

## 2. 시작하기

플러그인과 함께 시작하려면, 프로젝트에 플러그인을 적용해야합니다.

플러그인은 [그레이들 플러그인 포털에 게시되어있습니다.](https://plugins.gradle.org/plugin/org.springframework.boot) `plugins` 블록을 사용하여 적용할 수 있습니다.

```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2'
}
```

위의 plugin만 적용하면, 프로젝트에 큰 변화를 일으키지는 않습니다. 대신, 플러그인은 몇몇 다른 플러그인이 적용될 때, 이를 감지하고, 이에 반응합니다. 예를 들어, `java` 플러그인이 적용되면, 실행가능한 jar 파일로 빌드하는 TASK가 자동으로 설정됩니다. 일반적인 스프링부트 프로젝트는 최소한 [`groovy`](https://docs.gradle.org/current/userguide/groovy_plugin.html), [`java`](https://docs.gradle.org/current/userguide/java_plugin.html), [`org.jetbrains.kotlin.jvm`](https://kotlinlang.org/docs/reference/using-gradle.html) 플러그인을 적용합니다. 그리고, 의존성관리를 위해 [`io.spring.dependency-management`](https://github.com/spring-gradle-plugins/dependency-management-plugin) 플러그인 또는, 그레이들 네이티브 bom 지원을 적용합니다.

```groovy
apply plugin: 'java'
apply plugin: 'io.spring.dependency-management'
```

스프링부트 플러그인이 다른 플러그인이 적용될 때, 어떻게 동작하는지 배우려면, [다른 플러그인에 반응](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/reference/htmlsingle/#reacting-to-other-plugins) 의 장을 알아보세요.

<br>

## 3. 의존성 관리하기

스프링부트 어플리케이션에 의존성을 관리하기 위해서, [`io.spring.dependency-management`](https://github.com/spring-gradle-plugins/dependency-management-plugin) 플러그인을 적용하거나, 그레이들 네이티브 bom 지원을 사용할 수 있습니다. 전자의 주요한 장점은 프로퍼티 기반 버전을 수동으로 커스터마이제이션할 수 있는 것입니다. 후자를 사용하 때는 반대로, 더 이전 빌드 버전을 사용합니다.

<br>

### 3.1. 의존성관리 플러그인으로 의존성 관리하기

[`io.spring.dependency-management`](https://github.com/spring-gradle-plugins/dependency-management-plugin) 플러그인을 적용하면, 스프링부트의 플러인은 자동으로 여러분이 사용하는 버전의 [`spring-boot-dependencies` bom을 import 할 것입니다.](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/reference/htmlsingle/#reacting-to-other-plugins.dependency-management) 메이븐과 비슷한 의존성 관리 경험을 제공합니다. 예를 들어, bom에서 관리되는 의존성을 선언할 때, 버전 번호를 생략해도 됩니다. 이 기능을 사용하기 위해선, 일반적인 방식으로 의존성을 선언하고, 버전번호만 생략하면 됩니다.

```groovy
dependencies {
	implementation('org.springframework.boot:spring-boot-starter-web')
	implementation('org.springframework.boot:spring-boot-starter-data-jpa')
}
```

<br>

#### 3.1.1. 관리버전을 커스터마이징하기

 의존성관리 플러그인을 적용할 때, 자동으로 import 되는 `spring-boot-dependencies` bom은 의존성 버전을 관리하기 위해 프로퍼티를 사용합니다. 전체 프로퍼티 목록을 원한다면, 스프링부트 레퍼런스의 [`의존성 버전 부록`](https://docs.spring.io/spring-boot/docs/2.5.2/reference/htmlsingle/#dependency-versions-properties) 을 찾아보세요.

 관리버전을 커스터마이징하고 싶다면, 그에 상응하는 프로퍼티를 설정하세요. 예를 들어, SLF4J의 버전을 커스터마이징 하려면 아래와 같이 설정합니다.

```groovy
ext['slf4j.version'] = '1.7.20'
```

> ## 경고
>
> 각 스프링부트 출시버전은 특정 서드파티 의존성 집합에 맞게 설계되고, 테스트됩니다. 버전을 오버라이딩하는 것은 호환성 문제를 야기할 수 있으므로 주의해야합니다.

<br>

#### 3.1.2. 스프링부트 의존성관리를 단독으로 사용하기

스프링부트의 의존성관리는 스프링부트 플러그인을 적용하지 않고도 프로젝트 내에서 사용가능합니다. 

`SpringBootPlugin` 클래스는 `BOM_COORDINATES` 상수를 제공합니다. `BOM_COORDINATES` 상수는 그룹ID, 아티팩트ID, 버전을 알지 않아도 bom을 import 할 때 사용될 수 있습니다.

첫째, 스프링부트 플러그인에 의존하도록 프로젝트를 설정합니다. 그러나, 플러그인을 적용하지는 않습니다.

```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2' apply false
}
```

의존성관리 플러그인 위의 스프링부트 플러그인 의존성은 의존성관리 플러그인을 선언하지 않고도 사용할 수 있다는 것을 의미합니다. 이는 또한 여러분이 자동으로 스프링부트가 사용하는 것과 동일한 버전의 의존성관리 플러그인을 사용한다는 것을 의미합니다.

 의존성관리 플러그인을 적용하고, 이것을 스프링부트의 bom을 import하도록 의존성관리 플러그인을 설정하세요.

```groovy
apply plugin: 'io.spring.dependency-management'

dependencyManagement {
	imports {
		mavenBom org.springframework.boot.gradle.plugin.SpringBootPlugin.BOM_COORDINATES
	}
}
```

<br>

#### 3.1.3. 더 많은 것을 배우기

의존성관리 플러그인에 대해 더 많이 알고싶다면, 이것에 관한 [문서](https://docs.spring.io/dependency-management-plugin/docs/current/reference/html/)를 참조하세요.

<br>

### 3.2. 그레이들 Bom 지원으로 의존성을 관리하기

`생략`

<br>

## 4. 실행가능한 아카이브파일로 패키징하기

그레이들 플러그인은 어플리케이션의 의존성파일들을 포함하여 실행가능한 아카이브파일을(jar파일과 war파일 형태로) 생성할 수 있습니다. 그리고, `java -jar` 명령어로 실행할 수 있습니다.

<br>

## 4.1. 실행가능한 Jar파일로 패키징

실행가능한 jar파일은 `bootJar` task를 사용하여 빌드될 수 있습니다. `bootJar` task는 `java` 플러그인이 적용되고, [`BootJar`](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/api/org/springframework/boot/gradle/tasks/bundling/BootJar.html) 의 인스턴스일 때, 자동으로 생성됩니다. `assemble` task는 자동으로 `bootJar` task에 의존하도록 설정됩니다. 그렇게 해서, `assemble` (또는 `build`) task를 실행하면, `bootJar` task를 실행할 것입니다.

<br>

## 4.2. 실행가능한 War파일로 패키징

실행가능한 jar파일은 `bootWar` task를 사용하여 빌드될 수 있습니다. `bootWar` task는 `war` 플러그인이 적용되고,  [`BootWar`](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/api/org/springframework/boot/gradle/tasks/bundling/BootWar.html) 의 인스턴스일 때, 자동으로 생성됩니다. `assemble` task는 자동으로 `bootJar` task에 의존하도록 설정됩니다. 그렇게 해서, `assemble` (또는 `build`) task를 실행하면, `bootJar` task를 실행할 것입니다.

<br>

## 4.3. 실행가능한 일반 아카이브파일로 패키징

생략

## 4.4. 실행가능한 아카이브 패키징 설정하기

[`BootJar`](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/api/org/springframework/boot/gradle/tasks/bundling/BootJar.html) 와 [`BootWar`](https://docs.spring.io/spring-boot/docs/2.5.2/gradle-plugin/api/org/springframework/boot/gradle/tasks/bundling/BootWar.html) task는 각각 그레이들의 `Jar` 와 `War` task의 하위클래스입니다.

결과적으로 `jar와 war파일 패키징할 때` 사용가능한 표준설정옵션은 `실행가능한 jar와 war파일` 패키징할 때도 사용가능합니다. 또한, `실행가능한 jar와 war파일`에 한정되는 설정옵션들도 제공됩니다.

<br>

### 4.4.1. 메인클래스를 설정하는 것

기본설정은, 실행가능한 아카이브파일의 메인클래스는 자동으로 task의 클래스패스 상의 디렉토리들에서 `public static void main(String[])` 메소드를 가지는 클래스로 설정될 것입니다.

 메인클래스는 또한 task의 `mainClass` 프로퍼티를 이용하여 명시적으로 설정될 수 있습니다.

```groovy
bootJar {
	mainClass = 'com.example.ExampleApplication'
}
```

또 다른 방법으로, 스프링부트 DSL의 `mainClass` 프로퍼티를 이용하여 메인클래스 명을 프로젝트 전역으로 설정할 수 있습니다.

```groovy
springBoot {
	mainClass = 'com.example.ExampleApplication'
}
```

`이하 생략`

### 4.4.2. 개발단계에서만 의존성을 포함하기

기본설정은, `developmentOnly` 설정에 선언된 모든 의존성들은 `실행가능한 jar 또는 war` 파일에는 제외됩니다. 

 만약, `developmentOnly` 설정에 선언된 의존성들을 여러분의 아카이브에 포함하고 싶다면, task의 클래스패스를 설정합니다. `bootWar` task를 위한 예제는 다음과 같습니다.

```groovy
bootWar {
	classpath configurations.developmentOnly
}
```

### 4.4.3. 압축해제를 필요로하는 라이브러리 설정하기

`생략`

### 4.4.4. 아카이브를 완전 실행가능하게 만들기

`생략`

### 4.4.5. 계층이 있는 Jar와 War 패키징

