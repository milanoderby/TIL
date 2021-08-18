# Spring Legacy 프로젝트 생성하기

Maven으로도 동일한 과정을 거쳐서 Spring Legacy 프로젝트를 생성할 수 있지만, 저는 Gradle을 사용하겠습니다.

### Gradle Project 생성

좌측 상단 `File` - `New` - `Project` 클릭

![image-20210819024808582](./images/create-spring-project-1)

<br>

`Gradle` 선택 - `Next`클릭 - 기본 프로젝트 설정 후, 생성

![image-20210819030225851](./images/create-spring-project-2)

<br>

### Gradle에 Spring 의존체 추가

`Maven Repository`에서 추가할 Spring 의존체 검색 후, 추가

![image-20210819032904043](./images/create-spring-project-3)

<br>

위에서 검색한 의존체를 `build.gradle` 파일 - `dependencies` 에 추가

![image-20210819035940663](./images/create-spring-project-4)

<br>

우측 `Gradle 탭` - `Reload` 버튼을 클릭하여 `build.gradle` 파일에 입력된 의존체 추가 명령어를 실제 반영

클릭하면, 좌측 `Project 탭` - `External Libraries` 디렉토리에 의존성 라이브러리들이 추가됨

![image-20210819040047538](./images/create-spring-project-5)

<br>

수동으로 버튼을 클릭하면 불편하니, 자동으로 `build.gradle` 파일의 변경사항을 감지 및 `auto reload`하는 기능을 추가해보겠습니다.

우측 Gradle 탭의 `도구` 버튼 - `Auto-Reload Settings` 클릭

![image-20210819040604834](./images/create-spring-project-6)

<br>

아래 팝업창처럼 Build script가 `Any changes` 를 감지할 때, 자동으로 Reload하도록 설정

![image-20210819041241312](./images/create-spring-project-7)

<br>

### Gradle Build Script 작성

프로젝트 내 `build.gradle` 파일 작성 (추가된 설정들은 설명을 작성하였습니다.)

```groovy
plugins {
    id 'java'
    // application 플러그인 추가
    id 'application'
}

group 'com.madcoder'
version '1.0-SNAPSHOT'

// Java 소스의 버전을 명시
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

// 실행할 어플리케이션을 명시
application {
    mainClass = 'com.madcoder.practice.PracticeApplication'
}

dependencies {
    // spring-webmvc 의존체 추가 (내부적으로 core, aop, beans, context 등을 의존하기 때문에 함께 설치됩니다.)
    implementation 'org.springframework:spring-webmvc:5.2.3.RELEASE'

    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
}

test {
    useJUnitPlatform()
}
```

<br>

### Application 실행

저는 *Spring5 레시피* 교재에 나오는 Spring Bean 생성 및 출력에 대한 Java 예제코드를 작성하였습니다.

Application 실행 방법: 우측 `Gradle 탭` - `Tasks` - `application` - `run` 더블클릭

![image-20210819061915598](./images/create-spring-project-8)

<br>

### 참고자료

https://www.jetbrains.com/help/idea/work-with-gradle-projects.html#gradle_refresh_project

https://docs.gradle.org/current/samples/sample_building_java_applications.html
