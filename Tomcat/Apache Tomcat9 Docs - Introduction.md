# Apache Tomcat9 Docs - Introduction

## 용어

Context - 쉽게 말해, `Web Application`을 의미한다.



## 디렉토리와 파일구조

### bin

- `startup.sh`, `shutdown.sh` Tomcat 기동 / 정지 스크립트와 다른 스크립트들이 있다. (Windows에서 확장자는 .bat이다.)

### conf

- 설정파일들이 있고, 가장 중요한 설정파일은 `server.xml` 파일이다.

### logs

- 기본적으로 log 파일들이 있다.

### webapps

- 배포한 Web application 들이 있다.



## CATALINA_HOME 과 CATALINA_BASE

###  CATALINA_HOME

Tomcat 설치가 되는 root 디렉토리를 의미한다. ex) `/home/tomcat/apache-tomcat-9.0.10`을 의미한다.

### CATALINA_BASE

특정 Tomcat 인스턴스의 실행설정파일의 root 디렉토리를 의미한다.

하나의 서버(machine)에 여러 개의 Tomcat 인스턴스들을 가지고 싶다면, `CATALINA_BASE` 값을 사용해라.

### 

위의 2개의 값을 다르게 설정하면, `CATALINA_HOME` 은 jar파일이나, binary파일과 같은 정적자원들을 포함하게 될 것이고, `CATALINA_BASE` 경로에는 설정파일, 로그파일, 배포된 application 파일, 실행시간(runtime)에 필요한 파일들이 포함될 것이다.



### 왜 CATALINA_BASE를 사용하나

기본적으로, 2개의 값은 같은 디렉토리를 가리키도록 설정되어 있다. 하나의 서버에서 여러개의 Tomcat인스턴스들을 기동하고 싶을 때, CATALINA_BASE를 수동으로 설정한다. 이로 인해서 얻는 효과는 다음과 같다.

- 새로운 Tomcat 으로 업그레이드 하는것이 더 쉽게 관리된다. 여러개의 Tomcat 인스턴스들이 같은 CATALINA_HOME 경로를 바라보고 있고, 해당 경로에 있는 `jar파일`과 `binary 파일`들만 새로운 버전으로 업그레이드하면 되기 때문이다.
- 불필요한 jar파일들의 중복을 방지한다.
- `setenv.sh` 파일 또는 `bat script` 파일과 같은 특정 설정들을 공유할 수 있다.



### CATALINA_BASE 파일 내용

CATALINA_BASE 를 사용하기 이전에, 아래와 같은 디렉토리 구조를 가지도록 한다. 만약, `아래에서 생성을 권장한 디렉토리(recommended)` 디렉토리를 생성하지 않는다면, Tomcat이 자동적으로 디렉토리들을 만들긴 하겠지만, 만약 디렉토리 생성권한이 없다면, 문제가 발생하여 Tomcat 기동이 실패하고, 결국엔 제대로 작동하지 않을 수도 있다. 아래에 나오는 `조회규칙` 항목은 `CATALINA_BASE 디렉토리`와 `CATALINA_HOME 디렉토리` 두 경로 모두에 아래 파일들이 있다면, 어떠한 규익으로 조회하는지를 알려주는 항목이다.

#### bin 디렉토리

- `setenv.sh`파일과 `tomcat-juli.jar` 파일들을 포함하는 디렉토리 
- `디렉토리 생성 권장여부:` X
- `조회규칙`: CATALINA_BASE 경로 먼저 조회, 조회실패시, CATALINA_HOME 경로를 조회한다.

#### lib 디렉토리

- `classpath` 에 추가될 `resources` 파일들을 포함하는 디렉토리
- `디렉토리 생성 권장여부:` O
- `조회규칙`: CATALINA_BASE 경로 먼저 조회 후, CATALINA_HOME 경로도 조회한다.

#### logs 디렉토리

- `디렉토리 생성 권장여부:` O

#### webapps 디렉토리

- web application 들이 자동적으로 올려진다(load).
- `디렉토리 생성 권장여부:` O
- `조회규칙`: CATALINA_BASE 경로만 조회한다.

#### work 디렉토리

- 배포되는 web application을 위해 임시적으로 사용되는 디렉토리들
- `디렉토리 생성 권장여부:` O

#### temp 디렉토리

- JVM임시파일을 위해 사용된다.
- `디렉토리 생성 권장여부:` O



`tomcat-juli.jar` 파일은 가급적 바꾸지 않는 것을 권장한다. (tomcat 로깅 관련파일이기 때문에) 그러나, 특정 Tomcat Instance에서 자체 구현한 logging을 사용한다면, `CATALINA_BASE` 경로에 위치한 `tomcat-juli.jar` 파일을 교체할 수 있다.

`CATALINA_HOME/conf` 디렉토리에 있는 모든 설정파일들을 `CATALINA_BASE/conf` 디렉토리에로 복사하는 것을 권장한다. `CATALINA_BASE` 디렉토리에서 설정파일을 찾는데 실패하더라도, `CATALINA_HOME` 디렉토리를 조회하지 않기 때문에, Tomcat 기동 실패를 초래할 수 있다.



`CATALINA_BASE`는 최소한 다음 파일을 포함하고 있어야한다.

- `conf/server.xml`
- `conf/web.xml`



### CATALINA_BASE 를 사용하는 방법

`CATALINA_BASE` 는 환경변수이다.

Tomcat 기동 전에 환경변수를 아래 예시처럼 설정해준다.

On Unix: `CATALINA_BASE=/tmp/tomcat_base1_bin`



### Tomcat 설정 적용시기

모든 설정파일들은 Tomcat 기동 시, 읽힌다. 즉, 설정을 변경 및 적용하고 싶다면, Tomcat container를 재기동해야한다.