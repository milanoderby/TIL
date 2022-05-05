# WSL2 설치

### WSL (Windows Subsystem for Linux)이란?

과거에는 윈도우에서 Linux 를 사용하려면, Virtual box, VMware 등을 이용하여 환경을 구성하거나, OS 듀얼 부팅 설정이 필요했습니다.

하지만, 위와 같은 환경은 부팅 및 명령어 수행이 느릴 뿐만 아니라 자원 사용량도 불필요하게 많이 요구되기 때문에 좋은 방법은 아니었습니다.

Windows 에서는 이러한 개발자의 불편함을 인지하고, Windows 환경에서 Powershell을 Linux 환경의 Bash처럼 사용하고, 각종 명령어를 사용할 수 있게 WSL을 출시한 것입니다.

<br>

### WSL 설치 및 실행

#### 설치 가능한 패키지 확인

Command Prompt 창 또는 PowerShell 에서 아래 명령어를 입력합니다.

```powershell
wsl -l -o

# -l 은 --list
# -o 는 --online을 의미합니다.

# 실행결과
  NAME            FRIENDLY NAME
* Ubuntu          Ubuntu
  Debian          Debian GNU/Linux
  kali-linux      Kali Linux Rolling
  openSUSE-42     openSUSE Leap 42
  SLES-12         SUSE Linux Enterprise Server v12
  Ubuntu-16.04    Ubuntu 16.04 LTS
  Ubuntu-18.04    Ubuntu 18.04 LTS
  Ubuntu-20.04    Ubuntu 20.04 LTS
```

<br>

#### 설치할 배포판 지정하여 설치

Command Prompt 창 또는 PowerShell 에서 아래 명령어를 입력합니다.

PowerShell 실행 시, 관리자 권한으로 실행하여야 설치가 가능합니다.

```powershell
wsl --install -d <설치할 리눅스 배포명>

# wsl --install
# 위와 같이 -d 옵션은 명시하지 않으면, 기본 리눅스 배포버전인 Ubuntu로 설치됩니다.
```

PC를 다시 시작하면, 설치된 wsl 이 적용됩니다.

<br>

#### 구버전 Windows 환경에서 wsl --install이 지원되지 않을 때

`wsl --install` 명령어로 wsl이 설치되지 않을 수도 있습니다.

그 때는 아래를 참고하여 수동으로 wsl을 설치합니다.

https://docs.microsoft.com/ko-kr/windows/wsl/install-manual 를 참고하여 설치하면 됩니다.

<br>

#### wsl 실행

Windows 키 > wsl 검색 및 실행

또는

PowerShell 에서 `wsl` 입력

하는 방법으로 설치된 Ubuntu 를 시작할 수 있습니다.

<br>

### wsl 버전 확인

`wsl -l -v` 명령어를 이용하여 wsl2 가 정상적으로 설치되어 있는지 확인할 수 있습니다.

<br>

#### 디렉토리 구조

Windows 의 C 드라이브는 Ubuntu 에서 `/mnt/c` 로 마운트됩니다.

즉, Linux 작업 후, Windows 환경으로 작업결과를 이동시키려면, 위의 디렉토리를 이용하면 됩니다.

홈 디렉토리는 `/home/{username}` 입니다.

<br>

### 참고자료

https://docs.microsoft.com/ko-kr/windows/wsl/install

https://docs.microsoft.com/ko-kr/windows/wsl/setup/environment

https://docs.microsoft.com/ko-kr/windows/wsl/install-manual
