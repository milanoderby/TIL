# yum 을 이용한 OpenJDK 설치

### 1. 설치 가능한 OpenJDK 패키지 목록 확인

#### 설치 가능한 패키지 목록 확인 명령어

```shell
yum list [패키지명]
```

<br>

```shell
yum list java*jdk-devel
```

<br>

### 2. 원하는 버전의 패키지 설치

#### 패키지 설치 명령어

```shell
yum install [패키지명]
```

<br>

```shell
yum install java-1.8.0-openjdk-devel.x86_64
```