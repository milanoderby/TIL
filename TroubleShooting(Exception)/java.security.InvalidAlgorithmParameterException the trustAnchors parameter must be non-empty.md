# java.security.InvalidAlgorithmParameterException the trustAnchors parameter must be non-empty

### 문제

**HttpUrlConnection** 을 이용하여 서버와 HTTP 요청/응답을 받는 코드를 작성하여 실행했을 때, 발생

실제 배포된 서버에서는 발생하지 않는 오류이나, 로컬PC에서 구동시켰을 때, 발생

서버PC와 로컬PC의 어떠한 차이로 문제가 발생하는지에 대한 원인 파악이 필요

<br>

### 원인

#### jdk 버전의 차이로 인한 문제로 파악됨.

일반적인 jdk를 설치할 경우, https 요청 및 응답을 허용의 기준이 되는 인증서 저장소(Truststore)가 존재함.

<br>

#### Truststore 의 저장경로

{java 설치경로}/jre/lib/security/cacerts

<br>

#### 로컬 PC에서 사용 중인 Oracle jdk 1.7

중요한 것은 제가 설치한 jdk에서 **cacerts 파일**의 크기를 확인해보니 1KB 남짓이었다는 것이었습니다.

즉, https 요청/응답할 때, 해당 서버에 대한 인증정보가 존재하지 않기 때문에, 오류가 발생하는 것으로 보입니다.

구글링해보니, 정식으로 관리되고 있지 않은 jdk에서는 이와 같은 문제가 발생하는 것으로 보입니다.

![image-20220203135224637](./images/oracle-jdk-cacerts-file)

<br>

### 해결방법

현재까지도 관리되는 **Julu jdk 1.7 LTS 버전**을 로컬 PC에 재설치하였습니다.

cacerts 파일의 크기가 165KB 인 것을 보니, 인증된 서버의 정보를 제대로 관리 중인 것으로 보입니다.

![image-20220203135654678](./images/julu-jdk-cacerts-file)

<br>

### 참고자료

https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=lsmsky8584&logNo=220671615763

https://ethdemor.wordpress.com/2012/01/05/openjdk%eb%a1%9c-https-%ec%a0%91%ea%b7%bc%ec%8b%9c-the-trustanchors-parameter-must-be-non-empty-%ec%98%88%ec%99%b8-%eb%b0%9c%ec%83%9d/