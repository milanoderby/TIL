# Docker Nginx 설치 및 접속

WSL을 실행하여 아래 명령어를 순차적으로 실행합니다.

### 1. Docker 이미지 검색

```shell
docker search nginx
```

위의 명령어로 검색하면 `STARS`, `OFFICIAL` 등의 정보가 주어집니다. 이 중 중요한 지표는 `STARS`와  `OFFICIAL` 입니다.

`STARS` 의 개수가 많을 수록 추천한 사람이 많다는 의미이므로 신뢰할 수 있는 이미지입니다.

`OFFICIAL` , 즉 공식적으로 지원하는 버전의 이미지이므로 보안상으로 우수한 이미지입니다.

![image-20240217134705802](./images/search-result-of-docker-search-nginx)

<br>

사실, Docker 이미지 검색 자체는 [도커 허브](https://hub.docker.com) 에서 수행할 수도 있습니다.

검색결과는 https://hub.docker.com/search?q=nginx 에서 확인 가능합니다.

<br>

### 2. Docker 이미지 다운로드

도커 허브에서 원하는 이미지와 태그 값을 찾고, 해당 이미지를 다운로드합니다.

저는 Nginx 공식 이미지를 사용할 예정이므로 https://hub.docker.com/_/nginx/tags 에서 확인 후, 원하는 이미지를 다운로드 하는 명령어를 수행하였습니다.

```shell
docker pull nginx:latest
```

<br>

### 3. 다운로드한 Docker 이미지 조회

```nginx
docker images nginx
```

<br>

### 4. Docker 컨테이너 실행

Docker 이미지를 실행하면, 컨테이너라는 개념이 됩니다.

```shell
docker run -d -p 80: --name nginx_test nginx
```

여기서, 사용된 옵션은 아래와 같습니다.

- `-d (--detach)`: 컨테이너를 백그라운드에서 구동시킵니다. 옵션 생략 시, 컨테이너 내부에서 실행되는 애플리케이션의 상태가 화면에 계속 표시됩니다. Ctrl + C 를 누르면, 컨테이너가 중단되므로 서버나, DB를 실행하는 컨테이너는 반드시 옵션을 주어야합니다.
- `호스트에 80포트`와 `컨테이너의 80포트`를 연결해줍니다.
- `--name nginx_test` : 실행할 컨테이너의 이름을 명시합니다.

### 5. Docker 컨테이너 실행여부 확인

```shell
docker ps
```

<br>

### 6. Docker 컨테이너 bash 쉘 접속

```shell
docker exec -it nginx_test /bin/bash
```

<br>

### 참고 - Docker Desktop

사실, `2. Docker 이미지 다운로드` 까지 WSL에서 실행하고 나면, 이후 과정은 Docker Desktop 을 통해 편리하게 할 수 있습니다.