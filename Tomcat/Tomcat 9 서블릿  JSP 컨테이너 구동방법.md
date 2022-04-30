# Tomcat 9 서블릿 / JSP 컨테이너 구동방법

Tomcat 9은 JRE 8 이상 버전을 필요로 합니다.

1. JRE 8 이상 설치

   1. JRE 8 이상 다운로드 및 설치
   2. 또는, JDK 를 설치해도 됩니다.

2. Tomcat 다운로드 및 설치

   1. 바이너리 배포 다운로드
   2. 바이너리 배포를 `apache-tomcat-[version]` 형식의 이름을 가지는 디렉토리에 설치합니다. **CATALINA_HOME** 환경변수 값을 해당 디렉토리의 경로로 설정합니다.

3. 환경변수 설정

   Tomcat 은 Java Application 이고, 환경변수를 직접 사용하지 않습니다. Tomcat 구동 스크립트가 환경변수를 사용합니다. Tomcat 구동 스크립트는 Tomcat을 시작하는 명령어를 위해 환경변수를 사용합니다.

   1. CATALINA_HOME 과 CATALINA_BASE 환경변수 설정

      CATALINA_HOME 은 Tomcat 바이너리 배포의 root 디렉토리 경로로 설정해야됩니다. 추후, Tomcat 버전 업그레이드 및 유지보수를 위해서는 CATALINA_HOME 과 CATALINA_BASE 를 다른 값으로 사용하는 것을 권장합니다.

   2. 

<br>



### 참고자료

https://tomcat.apache.org/tomcat-9.0-doc/RUNNING.txt