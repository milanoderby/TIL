# grep 명령어

grep 명령어는 파일에서 패턴을 찾는 명령어입니다.

### 기본 문법

`grep [option] 패턴 [file...]`

<br>

### 정규표현식 활용

alphabet 단어를 포함하는 줄을 전체 출력하는 예시를 만들어보겠습니다.

예제의 입력으로 주어질 input.txt의 내용은 아래와 같습니다.

```
!@#$
1234test5678
%^&*
```

<br>

아래 명령어를 수행합니다.

```shell
grep -E '[a-zA-Z]+' input.txt

// 결과
1234test5678
```

<br>

### 참고자료

https://man7.org/linux/man-pages/man1/grep.1p.html