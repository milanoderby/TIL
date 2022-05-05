# Docker Desktop 설치

### Docker Desktop 설치하기 전에

상업적 사용자는 유료이지만, 개인 사용자는 무료로 사용할 수 있습니다.

저는 회사 이름으로 라이센스가 구매되어있기 때문에 Docker Desktop 을 설치하겠습니다.

Docker Desktop 을 설치하지 않고, WSL에서 Docker 를 설치하는 방법으로 우회도 가능합니다.

<br>

### 설치 전, 필요사항

WSL2 가 설치되어 있어야합니다.

<br>

### 설치방법

[Docker docs Docker Desktop 설치페이지](https://docs.docker.com/desktop/windows/install/) 에서 Docker desktop for Windows 를 설치합니다.

<br>

### 오류

`Docker Desktop requires the Server service to be enabled.` 

위와 같은 오류가 발생하여 수정합니다.

Windows 키 > service 검색 및 열기 > Server 더블클릭 > 시작유형을 `자동` 으로 변경

<br>

### 설치 확인

wsl2 에서 `docker --version` 명령을 치면, 도커가 설치되어있는지 확인할 수 있습니다.

<br>

### 참고자료

https://docs.docker.com/desktop/windows/install/

https://docs.microsoft.com/ko-kr/windows/wsl/tutorials/wsl-containers



