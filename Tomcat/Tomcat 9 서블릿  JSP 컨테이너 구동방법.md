# Tomcat 9 서블릿 / JSP 컨테이너 구동방법

Tomcat 9은 JRE 8 이상 버전을 필요로 합니다.

### Tomcat 구동방법

1. JRE 8 이상 설치

   1. JRE 8 이상 다운로드 및 설치
   2. 또는, JDK 를 설치해도 됩니다.

2. Tomcat 다운로드 및 설치

   1. 바이너리 배포 다운로드
   2. 바이너리 배포를 `apache-tomcat-[version]` 형식의 이름을 가지는 디렉토리에 설치합니다. **CATALINA_HOME 값**은 해당 디렉토리의 경로로 설정합니다.

3. 환경변수 설정

   Tomcat 은 Java Application 이고, 환경변수를 직접 사용하지 않습니다. Tomcat 구동 스크립트가 환경변수를 사용합니다. Tomcat 구동 스크립트는 Tomcat을 시작하는 명령어를 위해 환경변수를 사용합니다.

   1. CATALINA_HOME 과 CATALINA_BASE 환경변수 설정

      **CATALINA_BASE 값** 은 Tomcat 실제 설정을 저장하는 경로로 지정합니다. default 값은 CATALINA_HOME 값입니다.

      CATALINA_HOME 은 Tomcat 바이너리 배포의 root 디렉토리 경로로 설정해야됩니다. 추후, Tomcat 버전 업그레이드 및 유지보수를 위해서는 CATALINA_HOME 과 CATALINA_BASE 를 다른 값으로 사용하는 것을 권장합니다.

   2. JRE_HOME 또는 JAVA_HOME 환경변수 설정

      Tomcat을 구동할 때, 사용되는 JRE 또는 JDK 경로를 설정해야합니다.

      **JRE_HOME 값** 은 JRE의 경로를 지정할 때, 사용되는 값입니다.

      **JAVA_HOME 값** 은 JDK의 경로를 지정할 때, 사용되는 값입니다.

      이 환경변수는 **setenv 스크립트**에 저장하는 것을 권장합니다.

   3. 다른 환경변수 설정

      가장 자주 사용되는 환경변수는 **CATALINA_OPTS**입니다. 이 변수는 Tomcat을 구동하는 java 명령을 위한 추가 옵션입니다.

      Java Runtime Environment에 영향을 주는 옵션을 확인하려면, Java 공식문서를 참고하세요.

      Tomcat 특화된 System Properties 값을 확인하려면, Tomcat 설정의 System Properties 페이지를 확인하세요.

      CATALINA_OPTS와 비슷한 설정은 JAVA_OPTS 이며, 비교적 덜 사용합니다. Tomcat 구동 및 중지할 때, 모두 사용되는 옵션일뿐만 아니라, 다른 명령에도 적용되는 옵션입니다.

      **메모리 제한을 지정할 때, JAVA_OPTS 를 사용하지 마십시오.** Tomcat을 중지 시, 사용되는 프로세스는 그렇게 많은 메모리를 필요로 하지않습니다. **메모리 제한 설정은 CATALINA_OPTS 환경변수를 이용하여 설정하세요.**

   4. setenv 스크립트 이용

      **CATALINA_HOME 과 CATALINE_BASE 제외하고** 모든 환경변수는 **setenv 스크립트** 에 명시할 수 있습니다. 해당 스크립트는 `CATALINA_HOME/bin` 또는 `CATALINA_BASE/bin` 경로에 위치합니다.

      기본적으로 setenv 스크립트는 존재하지 않습니다.

      cATALINA_BASE/bin 와 CATALINA_HOME/bin 경로에 setenv 스크립트 파일이 존재한다면, CATALINA_BASE 경로가 먼저 적용됩니다.

      **CATALINA_HOME 과 CATALINE_BASE 환경변수는 setenv 스크립트 파일 위치를 찾을 때 사용되기 때문에 setenv 스크립트 파일에 명시하면 안됩니다.**

4. Tomcat 구동

   1. 아래 명령어를 수행하여 Tomcat 을 구동할 수 있습니다.

      `$CATALINA_HOME/bin/startup.sh`

      또는

      `$CATALINA_HOME/bin/catalina.sh start`

   2. 구동 후, Tomcat 기본 웹 어플리케이션은 아래 사이트에 접근하면 됩니다.

      `http://localhost:8080/`

5. Tomcat 중지

   1. 아래 명령어를 수행하여 Tomcat 을 구동할 수 있습니다.

      `$CATALINA_HOME/bin/shutdown.sh`

      또는

      `$CATALINA_HOME/bin/catalina.sh stop`

<br>

### Tomcat 인스턴스 여러 개 구동

많은 상황에서, 동일한 서버의 여러 사용자에 의해 공유되는 Tomcat 바이너리 배포의 사본을 가지는 것은 권장됩니다. 이것이 가능하려면, 개별 Tomcat 인스턴스를 위한 파일들을 포함하는 디렉토리 경로를 CATALINA_BASE 환경변수로 설정해야 됩니다. 

CATALINA_HOME 과 CATALINA_BASE 를 분리하여 실행할 때, 파일과 디렉토리는 아래와 같이 분리됩니다.

#### CATALINA_BASE 하위 디렉토리

- bin: 오직 setenv 스크립트, tomcat-juli.jar 파일(Logging 할 때, 사용되는 라이브러리) 만 사용됩니다.
- conf: **server.xml 파일**을 포함하는 서버 설정 파일
- lib: 라이브러리와 클래스파일
- logs: log 및 산출파일
- webapps: 구동 시, 메모리에 올라가는 web application
- work: web application을 위한 임시 작업 디렉토리
- temp: java.io.tmpdir 등의 임시파일을 위해 JVM에서 사용하는 디렉토리

<br>

#### CATALINA_HOME 하위 디렉토리

- bin: startup, shutdown 스크립트, CATALINA_BASE/bin 디렉토리에 setenv 스크립트, tomcat-juli.jar 파일이 없다면 CATALINA_HOME/bin 디렉토리에 있는 파일이 사용됩니다.
- lib: 라이브러리와 클래스파일 (CATALINA_BASE/lib 과 CATALINA_HOME/lib 하위 JAR 라이브러리와 클래스들이 공통 클래스패스에 추가됩니다. CATALINA_BASE/lib 하위 파일들이 먼저 추가됩니다.)

<br>

**각각의 web application 사이에는 라이브러리 공유하는 것을 권장하지 않습니다. 대신, 각각의 라이브러리는 application 내의 /WEB-INF/lib 디렉토리에 추가하세요.**

Tomcat에 의해 처리되는 XML conf 파일에서 CATALINA_HOME 과 CATALINA_BASE 환경변수 값은 ${catalina.home} 과 ${catalina.base} 를 이용하여 참조할 수 있습니다.

<br>

### 참고자료

https://tomcat.apache.org/tomcat-9.0-doc/RUNNING.txt 