# Docker 이미지 빌드 및 컨테이너 실행

#### Docker 이미지 Build

도커 이미지를 Build 합니다.

```shell
docker build -t spring-boot-sample:v0.0.1 -f Dockerfile .
```

`-t`: 도커 이미지에 이름과 태그를 명시하는 옵션입니다. (--tag의 약자)

`-f`: 도커 이미지 Build 시, 사용할 Dockerfile 을 명시하는 옵션입니다. (--file의 약자)

`맨 마지막에 붙는 .`: `Dockerfile` 내에서 기본적으로 사용할 `외부 경로(컨텍스트)`를 의미합니다. 주로, 컨테이너 외부에서 내부로 파일을 복사할 때 사용하는 컨테이너 외부의 기본 경로가 됩니다.

<br>

#### Docker 이미지 조회

위에서 빌드한 이미지를 조회합니다.

```shell
docker image ls
```

<br>

#### Docker 컨테이너 실행

빌드한 이미지를 이용하여 컨테이너를 실행할 수 있습니다.

```shell
docker run -d -p 8000:5000 spring-boot-sample:v0.0.1
```

`-d`: 컨테이너를 백그라운드에서 실행되도록 하는 옵션입니다. (--detach의 약자)

`-p`: 컨테이너 외부(호스트)의 포트를 컨테이너 내부의 포트로 바인딩하는 옵션입니다. (--publish의 약자)

여기서는 호스트의 8000번 포트 호출 시, 컨테이너의 5000번 포트로호출이 전송되도록 하였습니다.

`-p` 옵션을 명시하지 않았다면, 컨테이너는 네트워크를 포함하여 격리되기 때문에 접근할 수 없게 됩니다.

`spring-boot-sample:v0.0.1` 는 컨테이너로 실행할 Docker 이미지의 이름을 의미합니다.

<br>

#### Docker 컨테이너 조회

실행 중인 컨테이너를 조회합니다.

```shell
docker ps
```

<br>

실행 중이지 않은 컨테이너도 포함해서 확인하고 싶다면, `-a` 옵션을 추가합니다. (-all 의 약자)

```shell
docker ps -a
```

<br>

결과는 아래와 같습니다.

```
# USAGE: docker ps [OPTIONS]
docker ps -a                       
CONTAINER ID        IMAGE                 COMMAND                CREATED             STATUS              PORTS                                       NAMES
a02b319ef343        flask-sample:v0.0.1   "python ./server.py"   21 minutes ago      Up 21 minutes       0.0.0.0:8000->5000/tcp, :::8000->5000/tcp   elated_brahmagupta
```

<br>

#### Docker 컨테이너 중지

docker run 명령어 실행 시, 별도의 `--name` 옵션을 주지 않았다면 임의의 이름을 자동 지정합니다.

따라서, 위에서 실행한 컨테이너 조회 명령어로 확인한 이름을 파라미터로 이용하여 컨테이너를 중지해야합니다.

```shell
docker stop [컨테이너 이름]
```

<br>

#### Docker 컨테이너 제거

컨테이너를 완전히 제거하여 공간을 확보합니다.

```shell
docker rm [컨테이너 이름]
```

<br>

#### 실행 중인 Docker 컨테이너에 접근

실행 중인 컨테이너 내부에서 bash 명령어를 수행하는 방법은 아래와 같습니다.

```shell
docker exec -it [컨테이너 이름] bash
```

<br>

#### Docker 컨테이너 실행 시, 지정한 명령어 대신 다른 명령어 수행

컨테이너 실행 시, 제대로 동작하지 않을 때, Dockerfile에서 지정한 명령어 대신 bash 명령어를 수행하여 컨테이너 내부에 직접 접근하기 위해 아래 명령어를 수행합니다.

```shell
docker run -it --rm [컨테이너 이미지 이름] bash
```

<br>