# Linux 파티션 용량 재할당 (home 디렉토리에 할당된 용량 root 디렉토리에 재할당하기)

### TL;DR

적절한 참고자료를 찾았으나, NCloud 환경에서 잘 되지 않아서 실제로 완료하지 못했습니다.

<br>

수동으로 디스크 파티셔닝 설정을 하지 않았다면, `/` 디렉토리에 할당되는 디스크 용량은 `/home` 디렉토리에 할당된 용량보다 적을 것입니다. (CentOS 초기 설치 시, 별도 설정이 없을 때)

<br>

중요한 것은 사용하다보면, home 디렉토리보다는 root 디렉토리의 용량이 부족해진다는 것입니다.

그렇기 때문에 home 디렉토리에서 사용하지 않는 용량을 root 디렉토리로 재할당해보겠습니다.

0. 디스크 할당된 용량 확인

   `df -h`

   ![image-20220514222045432](./images/disk-partition-reallocation-1)

   위와 같이 `/` 디렉토리에 마운트된 용량이 `/home` 디렉토리에 마운트된 용량보다 적으며, `/`디렉토리의 사용률은 매우 높은 상황일 때, 디스크 용량 재할당을 합니다.

1. `/home` 디렉토리 언마운트

   `sudo umount /home`

   ![image-20220514222432823](./images/disk-partition-reallocation-2)

   위와 같은 오류가 발생한다면, 해당 디렉토리가 특정 프로세스에 의해 사용되고 있음을 의미합니다. 디렉토리를 사용하는 프로세스를 강제로 죽이겠습니다.

2. 디스크 사용 중인 프로세스 강제 종료

   `fuser -ck /home`

   fuser: command not found 오류가 발생한다면, 설치가 되어있지 않다는 의미이므로 해당 명령어 사용을 위해 패키지를 설치합니다.

   `sudo yum -y install psmisc`

3. 

<br>

### 참고자료

- https://knoow.tistory.com/179

- https://gist.github.com/troyfontaine/87091bd6a5c68f45dd62ced3d12bc377
