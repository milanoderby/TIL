# if 문

### 기본형 - script 에서 사용가능, shell에서 사용할 때는 이스케이프 문자를 주의하여 사용해야 됩니다.

아래 3가지 문법 중 하나를 사용하면 됩니다.

```shell
if test <조건명령어>; then <수행할 명령어> ; fi
if [ <조건명령어> ]; then <수행할 명령어> ; fi
if [[ <조건명령어> ]]; then <수행할 명령어> ; fi
```

<br>

### 조건명령어

자주 사용하는 조건명령어만 정리합니다.

| 옵션                     | 설명                                                         |
| ------------------------ | ------------------------------------------------------------ |
| -f <file\>               | file 이 존재하고, file 유형이 file일 때 (file유형의 종류: file, directory, block 등), true를 반환합니다. |
| 예시                     | if [ -f "/home1/madcoder/test.txt" ]; then echo "file exists"; fi<br />if [ -f $FILEPATH ]; then echo "file exists"; fi |
| -d <file\>               | file 이 존재하고, file 유형이 directory일 때 (file유형의 종류: file, directory, block 등), true를 반환합니다. |
| -z <string\>             | string 의 길이가 0일 때(zero), true를 반환합니다.            |
| -n <string\>             | string 의 길이가 0이 아닐 때(non-zero), true를 반환합니다.   |
| <string1\> == <string2\> | 두 문자열이 같을 때, true를 반환합니다.                      |
| <string1\> != <string2\> | 두 문자열이 다를 때, true를 반환합니다.                      |
| <string1\> < <string2\>  | 사전적으로 string1이 string2보다 앞설 때, true를 반환합니다. |
| 산술연산자               | 변수를 문자열이 아닌 숫자로 보고, 산술연산자를 수행할 때, 사용합니다.<br />`-eq` `-ne` `-lt` `-le` `-gt` `-ge` 등이 있습니다. |
| 부정연산자               | !                                                            |
| AND 연산자               | -a                                                           |
| OR 연산자                | -o                                                           |

자주 사용하는 예시는 아래 참고자료를 확인하세요.

<br>

### 참고자료

https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html

https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html

https://linuxize.com/post/bash-check-if-file-exists/

https://www.geeksforgeeks.org/conditional-statements-shell-script/