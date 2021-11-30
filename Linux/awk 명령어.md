# awk 명령어

awk 프로그래밍 언어를 사용하면 로그와 관련하여 편리하게 작업할 수 있습니다.

필요한 명령어를 때에 맞게 남겨놓습니다.

<br>

### 상황에 따라서 사용하는 awk 명령어

#### 특정 시간 범위 내의 로그들만 콘솔에 출력

```shell
awk '"16:29:34" <= $1 && $1 <= "16:31:44"' catalina.out.2021-11-30
```

<br>

#### 특정 시간 범위 내의 로그들을 파일로 추출

```shell
awk '"16:29:34" <= $1 && $1 <= "16:31:44"' catalina.out.2021-11-30 > test.txt
```

<br>

#### 배포시점이 다른 2개의 로그파일을 추출하고, 비교

```shell
awk '{ if ("17:31:43" <= $2 && $2 <= "17:33:20") print substr($0, index($0,$4)) }' catalina.out.2021-11-23 > old_deploy.tx
awk '{ if ("17:59:47" <= $1 && $1 <= "18:01:51") print substr($0, index($0,$4)) }' catalina.out.2021-11-30 > new_deploy.txt
```

1열은 연월일입니다.

2열은 시간입니다.

시간 값의 비교를 제외하기 위해서 **3열부터 마지막 열까지만 추출**해야합니다.

이를 위해 index와 substr 함수를 사용하였습니다.

index를 통해서 전체 문자열($0)에서 4열($4)의 문자가 시작되는 지점(index)을 찾습니다.

위에서 찾은 전체 문자열($0)에서 index 지점부터 이후의 문자열들을 모두 추출하는 substr 함수를 사용합니다.

위와 같은 작업으로 4열부터 문자열이 추출됩니다.

<br>

### awk 내장 함수

index와 substr 함수의 사용예시를 쉽게 파악하기 위해 다음 링크를 참고합니다.

https://www.geeksforgeeks.org/built-functions-awk/

<br>

### 참고자료

https://recipes4dev.tistory.com/171

https://stackoverflow.com/questions/13975493/log-file-search-based-on-date-and-time-range-on-multiple-log-files-using-awk?rq=1