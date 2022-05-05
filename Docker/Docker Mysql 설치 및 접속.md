# Docker Mysql 설치 및 접속

wsl2 를 이용하여 아래 명령어를 수행합니다.

<br>

### Mysql 설치

1. mysql 컨테이너 이미지 검색

   `docker search mysql`

2. 컨테이너 이미지 다운로드

   `docker pull mysql`

   아무런 tag 값을 주지 않았기 때문에 default 값인 latest 태그 버전으로 설치됩니다.

3. 다운로드한 이미지 조회

   `docker images mysql`

4. 컨테이너 실행

   `docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password --name mysql_test mysql`

   여기서, 사용된 옵션은 아래와 같습니다.

   - -d (--detach): 컨테이너를 백그라운드에서 구동시킵니다. 옵션 생략 시, 컨테이너 내부에서 실행되는 애플리케이션의 상태가 화면에 계속 표시됩니다. Ctrl + C 를 누르면, 컨테이너가 중단되므로 서버나, DB를 실행하는 컨테이너는 반드시 옵션을 주어야합니다.
   - 호스트에 3306포트와 컨테이너의 3306포트를 연결해줍니다.
   - -e MYSQL_ROOT_PASSWORD=password: 컨테이너를 생성하면서, 환경변수를 지정합니다.
   - --name mysql_test: 실행할 컨테이너의 이름을 명시합니다.

5. 컨테이너 실행여부 확인\

   `docker ps`

6. 컨테이너 bash 쉘 접속

   `docker exec -it mysql_test /bin/bash`

7. mysql 서버 접속

   `mysql -u root -p`

   기본 계정은 root로 설정되어있고, password는 이전에 명시한 password 로 접속합니다.

<br>

### 참고자료

https://hub.docker.com/_/mysql