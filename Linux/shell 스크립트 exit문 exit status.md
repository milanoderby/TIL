# shell 스크립트 exit문 exit status, return code

### 사용법

shell 스크립트 에서 exit 문을 사용하면 스크립트가 바로 종료되고, 종료코드(exit status)를 반환할 수 있습니다.

함수에서는 return code를 반환할 수 있습니다.

반환된 exit status, return code는 **$?** 명령어를 통해 확인 가능합니다.

<br>

### 사용예시

#### test.sh 파일

```shell
#!/bin/bash

print_hello() {
    echo "hello world"
    return 0
}

print_error() {
    echo "error occured!"
    return 100
}

print_hello
echo $?

print_error
echo $?

exit 1
```

<br>

#### test.sh 파일 수행

```shell
./test.sh
echo $?

# 출력결과
"hello world"
0
"error occured!"
100
1
```

<br>

