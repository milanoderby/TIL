# zip 명령어

zip 명령어는 압축 및 포장 역할을 하는 명령어입니다.

`.zip` 확장자를 사용합니다.

<br>

### 사용법 - 압축

zip [options] <결과 파일명> <압축대상 파일목록...>

<br>

### 사용법 - 압축 해제

unzip [options] <압축해제할 파일명>

<br>

### 자주 사용하는 option

| option | 설명                                                         | 사용법 예시                         |
| ------ | ------------------------------------------------------------ | ----------------------------------- |
| -d     | 이미 생성된 zip파일 내의 파일들 중 하나를 삭제합니다.        | zip -d myfile.zip toDelete.txt      |
| -u     | 이미 생성된 zip파일에 파일을 추가합니다.                     | zip -u myfile.zip toAdd.txt         |
| -m     | 파일 압축 후, 압축된 원본파일들을 삭제합니다.<br />파일 압축이 정상종료된 후에만 파일 삭제를 진행합니다. | zip -m myfile.zip original*.txt     |
| -r     | 압축대상 파일을 directory로 명시할 때, <br />directory 하위 파일들도 recursive 하게 압축합니다. | zip -r myfile.zip directory_name    |
| -v     | 압축과정을 출력합니다.                                       | zip -v myfile.zip verbose.txt       |
| -P     | 압축 시, 암호화할 때, 사용할 패스워드를 명시합니다.          | zip -P password myfile.zip test.txt |

<br>

### 참고자료

https://www.geeksforgeeks.org/zip-command-in-linux-with-examples/

http://www.linuxguide.it/command_line/linux-manpage/do.php?file=zip
