# yum 으로 설치한 package 의 명령어가 동작하지 않을 때

yum install 로 설치했으나, lvm2 를 설치하였으나, lvm2 내부에 있는 lvremove 명령어를 사용하지 못해서 해당 오류를 조사하였습니다.

<br>

### yum 으로 설치된 패키지의 명령어가 위치한 디렉토리: /usr/sbin

확인해보니 yum으로 설치한 패키지의 명령어는 /usr/sbin의 위치하고 있었습니다.

<br>

### 해결방법

1. 사용할 명령어 앞에 디렉토리 명시

   `/usr/sbin/lvremove`

2. 환경변수에 디렉토리 추가

   `export PATH=$PATH:/usr/sbin`

<br>

### 참고자료

https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_PATH_%EC%B6%94%EA%B0%80