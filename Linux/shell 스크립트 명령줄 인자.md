# shell 스크립트 명령줄 인자

shell 스크립트 명령줄 인자를 출력하는 변수는 다양합니다.

### shell 스크립트 명령줄 인자가 맵핑되는 변수

| 변수                  | 설명                                                         |
| --------------------- | ------------------------------------------------------------ |
| $N (여기서, N은 숫자) | 실행한 명령어를 공백문자(space) 단위로 잘랐을 때, 분리된 각각의 인자를 순서대로 $0, $1, $2 ... $N 의 환경변수로 설정합니다. |
| $0                    | $0은 명령줄 인자가 아닌 shell 스크립트를 실행하기 위한 명령어로 맵핑됩니다.<br />c언어 main함수에서 인자로 받는 argc에 해당합니다. |
| $#                    | 명령줄 인자로 주어진 인자의 개수                             |
| $*                    | 명령줄 인자를 하나의 줄에 공백문자(space)로 구분하여 출력합니다. |
| $@                    | 명령줄 인자를 여러 줄에 개행문자(\n)로 구분하여 출력합니다.  |

<br>

### 예시

#### test.sh

```shell
#!/bin/bash

# total number of command-line arguments
echo "Total arguments passed are: $#"

# $* is used to show the command line arguments
echo "The arguments \$* are: $*"
for argv in "$*"
do
        echo $argv
done

# $* is used to show the command line arguments
echo "The arguments \$@ are: $@"
for argv in "$@"
do
        echo $argv
done

echo "The Command is: $0"
echo "The First Argument is: $1"
echo "The Second Argument is: $2"
echo "The Third Argument is: $3"
echo "The Fourth Argument is: $4"
```

<br>

#### test.sh 스크립트 수행

```shell
./test.sh 1 2 3 4

# 출력결과
Total arguments passed are: 4
The arguments $* are: 1 2 3 4
1 2 3 4
The arguments $@ are: 1 2 3 4
1
2
3
4
The Command is: ./test.sh
The First Argument is: 1
The Second Argument is: 2
The Third Argument is: 3
The Fourth Argument is: 4
```

<br>