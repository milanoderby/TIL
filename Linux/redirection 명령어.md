# redirection 명령어 (>, >>)

redirection 이란, 출력의 방향을 바꾼다는 의미입니다.

### > (덮어쓰기)

#### 사용 예시

`echo $HOSTNAME > [출력 파일]`

위와 같이 사용하면 HOSTNAME 환경변수를 출력파일의 기존 내용은 삭제되고, 그 위에 새로운 내용으로 덮어씁니다.

<br>

### >> (이어쓰기)

#### 사용 예시

`echo $HOSTNAME >> [출력 파일]`

위와 같이 사용하면 HOSTNAME 환경변수를 출력파일의 기존 내용의 뒤에 이어씁니다.