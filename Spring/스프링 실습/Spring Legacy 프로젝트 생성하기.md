# Spring Legacy 프로젝트 생성하기

Maven으로도 동일한 과정을 거쳐서 Spring Legacy 프로젝트를 생성할 수 있지만, 저는 Gradle을 사용하겠습니다.

### Gradle Project 생성

좌측 상단 `File` - `New` - `Project` 클릭

![image-20210819024808582](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-1)

<br>

`Gradle` 선택 - `Next`클릭 - 기본 프로젝트 설정 후, 생성

![image-20210819030225851](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-2)

<br>

### Gradle에 Spring 의존체 추가

`Maven Repository`에서 추가할 Spring 의존체 검색 후, 추가

![image-20210819032904043](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-3)

<br>

위에서 검색한 의존체 추가 명령어를 `build.gradle` 파일 - `dependencies` 에 추가

![image-20210819035940663](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-4)

<br>

우측 `Gradle 탭` - `Reload` 버튼을 클릭하여 `build.gradle` 파일에 입력된 의존체 추가 명령어를 실제 반영

클릭하면, 좌측 `Project 탭` - `External Libraries` 디렉토리에 의존성 라이브러리들이 추가됨

![image-20210819040047538](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-5)

<br>

수동으로 버튼을 클릭하면 불편하니, 자동으로 `build.gradle` 파일의 변경사항을 감지 및 `auto reload`하는 기능을 추가해보겠습니다.

우측 Gradle 탭의 `도구` 버튼 - `Auto-Reload Settings` 클릭

![image-20210819040604834](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-6)

<br>

아래 팝업창처럼 Build script가 `Any changes` 를 감지할 때, 자동으로 Reload하도록 설정

![image-20210819041241312](D:\git-repository\TIL\Spring\스프링 실습\images\create-spring-project-7)

<br>

### 참고자료

https://www.jetbrains.com/help/idea/work-with-gradle-projects.html#gradle_refresh_project
