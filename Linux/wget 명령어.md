# wget 명령어

### 사용법

wc [option] [대상 URL]

webget 이라는 단어를 줄인 명령어로 Web상의 리소스를 다운로드하는 명령어입니다.

<br>

### option

| 옵션           | 설명                                                         | 예시                                                |
| -------------- | ------------------------------------------------------------ | --------------------------------------------------- |
| -b             | 백그라운드에서 자원을 다운로드합니다.                        | wget -b https://www.naver.com/favicon.ico           |
| -o \<출력파일> | [대상URL] 에서 다운로드 받은 자원들을 <출력파일>에 저장합니다. | wget https://www.naver.com/favicon.ico -o icon-file |

<br>

### 참고자료

https://man7.org/linux/man-pages/man1/wget.1.html

https://sisiblog.tistory.com/225