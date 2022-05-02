# here strings (<<<)

`<<<` 명령어 뒤의 문자열을 `<<<` 앞에서 사용되는 명령어의 standard input 값으로 지정합니다.

### 사용예시

#### test.sh 파일

```shell
#!/bin/bash

cat <<< "Test!"
sed 's/a/b/g' <<< "aaa"
read first second <<< "hi there"
echo $second $first
```

<br>

#### test.sh 파일 수행

```shell
./test.sh

# 출력결과
Test!
bbb
there hi
```

<br>

### 참고자료

https://unix.stackexchange.com/questions/80362/what-does-mean

