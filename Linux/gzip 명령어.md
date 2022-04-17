# gzip 명령어

gzip 명령어는 압축 및 포장 역할을 하는 명령어입니다.

압축된 파일은 GNU zip 헤더와 deflated (압축된) 데이터를 포함합니다.

<br>

### 사용법 - 압축

 gzip [Options] [filename]

**압축대상 파일들을 인자로 받으면, 압축 후, `.gz` 확장자를 뒤에 붙이고, 압축대상 원본파일들은 삭제합니다.**

<br>

### 사용법 - 압축 해제

gzip -d <압축해제할 파일명>

<br>

### 자주 사용하는 option

| option | 설명                                                         | 사용법 예시                   |
| ------ | ------------------------------------------------------------ | ----------------------------- |
| -f     | `대상파일명.gz` 파일이 이미 존재할 경우, gzip 명령어 수행 시, 기존 파일을 덮어쓰기할지 말지를 물어봅니다.<br />-f 옵션은 force의 약자로 압축결과 파일명이 기존 파일과 겹칠 경우에 물어보지 않고, 덮어쓰기를 수행합니다. | gzip -f already_exist.txt     |
| -k     | gzip 압축 후, 원본파일을 삭제하지 않는 옵션입니다.           | gzip -k not_delete.txt        |
| -r     | 대상파일을 directory로 명시할 때, directory 하위 경로들을 recursive하게 순회하며, 파일을 압축합니다.<br />이 때, 모든 파일을 합쳐서 하나의 파일로 압축하는 것이 아닌 각각의 파일들을 하나씩 압축합니다. | gzip -r test_directory        |
| -[1-9] | 압축 레벨을 정하는 옵션입니다.<br />숫자가 낮을 수록 빠르고, 덜 압축됩니다. | gzip -1 compression_level.txt |
| -v     | 압축  진행과정을 출력합니다.                                 | gzip -v print.txt             |
| -d     | gzip으로 압축된 파일을 압축 해제합니다.                      | gzip -d compressed.txt        |

<br>

### 참고자료

https://www.geeksforgeeks.org/gzip-command-linux/?ref=lbp