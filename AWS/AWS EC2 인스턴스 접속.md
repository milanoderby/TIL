# AWS EC2 인스턴스 접속

### 시작하기 전에...

저는 Windows 환경을 사용하므로 Linux 서버에 접속하기 위한 프로그램인 `Putty` 를 먼저 설치하였습니다.

[Putty 설치링크](https://www.putty.org/)

[AWS 가이드 문서](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html)를 참고하였습니다.

<br>

### 1. 인스턴스 가동상태 확인

인스턴스 상태가 `실행 중` 인지 확인합니다.

![image-20210801202926286](./images/aws-putty-1)

<br>

### 2. 보안그룹 - 방화벽 인바운드 규칙 확인

위의 화면에서 `인스턴스` 를 클릭하여 `인스턴스 상세화면`으로 이동합니다.

하위 `보안` 탭 클릭 후, 인바운드 규칙이 `접속하는 IP` 에 대해 `SSH 프로토콜`을 허용하는지 확인합니다.

SSH 프로토콜은 22번 포트를 사용하므로 아래와 같습니다.

![image-20210801201135511](./images/aws-putty-2)

<br>

### 3. Putty 실행 및 IP 입력

Putty HostName(IP address) 입력란에는 `인스턴스 상세화면` 에 나와있는 서버의 `퍼블릭 IPv4 주소` 또는 `퍼블릭 IPv4 DNS` 를 입력합니다.

하지만, 위의 IP 입력 후, `Open` 버튼을 클릭하는 것으로 접속이 되지 않습니다.

왜냐하면, 인스턴스 생성 시, RDP(원격 데스크톱 프로토콜)을 위해 키페어를 생성하였기 때문입니다.

![image-20210801202317755](./images/aws-putty-3)

<br>

### 4. Putty Private Key 변환

인스턴스 생성 시, 다운로드 받은 개인키(Private Key)의 확장자에 따라 사용할 수 있는 방식이 달라집니다.

`.pem` 파일은 `OpenSSH`에서 사용할 수 있는 형식이며, `.ppk` 파일이라면, `Putty` 에서 사용가능한 파일형식입니다.

만약, 개인키 파일을 `.ppk` 파일로 다운로드 받았다면, 이 과정이 필요없습니다. 개인키 파일확장자가 `.pem`이라면 이를 `ppk`로 변환해야합니다. 변환과정은 다음과 같습니다.

#### 1. `Puttygen` 프로그램을 실행합니다.

#### 2. `Load` 버튼 클릭하여 `.pem` 확장자의 개인키파일을 가져옵니다.

![image-20210801205833837](./images/aws-putty-4)

#### 3. `Parameters` 검토

Puutygen 프로그램 하단의 `Paramters` 란을 확인합니다.

![image-20210801210058996](./images/aws-putty-5)

`Type of key to generate` : RSA 로 설정합니다.

`Number of bits in a generated key` : 2048 로 설정합니다.

###### 왜냐면, Amazon EC2에서 사용되는 키는 2048비트 SSH-2 RSA 키이기 때문입니다.

(참고: https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html)

#### 4. Save private key 버튼 클릭

이제 Puttygen에서 `.pem` 파일을 `.ppk` 파일로 변환하여 저장합니다.

<br>

### 5. Putty Auth 설정

위에서 변환한 `Putty Private Key(.ppk)` 파일을 가져온 후, `Open`(접속) 버튼 클릭합니다.

1. `Putty 프로그램` > 좌측 `SSH` > 좌측 `Auth` > `Private key file for authentication` > `Browse` 로 `.ppk` 파일을 가져옵니다.
2. 하단 `Open` 버튼 클릭

![image-20210801211258013](./images/aws-putty-6)

<br>

### 6. Centos 접속

위의 과정을 통해 접속하면, 접속계정을 물어봅니다.

CentOs7 의 기본 접속계정명은 `centos`로 설정되어있기 때문에 입력하고, 접속합니다.

![image-20210801211934157](./images/aws-putty-7)
