# shift 명령어

### 사용법

shift [n]

shell 스크립트 실행 시, 명령줄로 주어진 인자가 있을 때, 이 인자들을 N칸씩 왼쪽으로 이동시키는 명령어입니다.

n 값은 생략 시, n의 기본 값은 1로 설정됩니다.

<br>

### 예시

#### test.sh 파일

```shell
#!/bin/bash

# 명령줄로 주어진 인자의 개수를 출력
echo "Total arguments passed are: $#"

# 명령줄로 주어진 인자들을 모두 출력
echo "The arguments are: $*"

# 명령줄로 주어진 인자 중 첫번째 인자를 출력
echo "The First Argument is: $1"

# shift 2 명령어 수행 후, 명령줄로 주어진 인자 중 첫번째 인자를 출력
shift 2
echo "The First Argument After Shift 2 is: $1"

# shift 명령어 수행 후, 명령줄로 주어진 인자 중 첫번째 인자를 출력
shift
echo "The First Argument After Shift is: $1"
```

<br>

#### test.sh 스크립트 수행

```shell
./test.sh 1 2 3 4

# 출력결과
Total arguments passed are: 4
The arguments are: 1 2 3 4
The First Argument is: 1
The First Argument After Shift 2 is: 3
The First Argument After Shift is: 4
```

<br>

### 참고자료

https://www.geeksforgeeks.org/shift-command-in-linux-with-examples/