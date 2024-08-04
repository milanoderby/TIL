# yum install 명령이 block시, 해결방법

### 문제현상

`yum install <패키지명>` 수행 시, 설치가 되지 않고, Blocking 되는 현상입니다.

<br>

### 해결방법

RPM data file을 삭제 및 재빌드

캐시 삭제 및 재생성

1. RPM data file 확인

   `ll /var/lib/rpm`

2. RPM data file 삭제

   `sudo rm -f /var/lib/rpm/__db.00*`

3. RPM data file 재빌드

   `rpm -vv --rebuilddb`

4. yum 캐시 삭제

   `sudo yum clean all`

5. yum 캐시 생성

   `yum makecache`

이후, 다시 yum install 명령을 다시 수행하면, 정상적으로 설치되는 것을 확인할 수 있습니다.

<br>

### 참고자료

https://developpaper.com/centos-yum-stuck-during-installation-solution/