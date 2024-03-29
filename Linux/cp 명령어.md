# cp 명령어

### 사용법

cp [option]... [복사할 파일] [복사될 경로]

파일, 디렉토리를 복사하는 명령어입니다.

**복사될 경로에 동일한 파일명이 존재할 경우, 덮어쓰기합니다.**

<br>

### option

| 옵션 | 설명                                                         |
| ---- | ------------------------------------------------------------ |
| -i   | 이미 복사할 파일과 동일한 파일명이 목적경로에 존재한다면, 사용자에게 덮어쓸지 물어봅니다. |
| -r   | 복사할 파일이 디렉토리일 경우, 해당 디렉토리 하위 파일들을 recursive 하게 목적경로로 복사합니다. |
| -v   | 복사하는 과정을 출력합니다.                                  |

<br>

### 참고자료

https://man7.org/linux/man-pages/man1/cp.1.html

