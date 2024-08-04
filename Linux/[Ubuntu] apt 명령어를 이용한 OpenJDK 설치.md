# [Ubuntu] apt 명령어를 이용한 OpenJDK 설치

### 1. 설치 가능한 OpenJDK 패키지 목록 확인

#### 설치 가능한 패키지 목록 확인 명령어

```shell
sudo apt search [패키지명]
```

<br>

```shell
sudo apt search openjdk-8
```

<br>

### 2. 원하는 버전의 패키지 설치

패키지를 설치하기 전에 아래 명령어를 수행하는 것이 좋습니다.

현재 다운로드 하려는 패키지 링크가 최신 패키지 배포판의 링크가 아닐 수도 있기 때문에 이를 갱신합니다.

```shell
sudo apt-get update
```

<br>

#### 패키지 설치 명령어

```shell
sudo apt install [패키지명]
```

<br>

```shell
sudo apt install openjdk-8-jdk
```