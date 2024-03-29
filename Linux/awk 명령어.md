# awk 명령어

awk 프로그래밍 언어를 사용하면 로그와 관련하여 편리하게 작업할 수 있습니다.

필요한 명령어를 때에 맞게 남겨놓습니다.

awk 는 기본적으로 아래의 프로세스를 가집니다.

1. 입력문자열 또는 파일을 받습니다.
2. 조건에 맞는 특정행을 필터링
3. 각 행별로 명령문을 수행합니다.

<br>

### 사용법

#### 문법

```shell
awk [-F sepstring] [-v assignment]... program [argument...]
awk [-F sepstring] -f progfile [-f progfile]... [-v assignment]... [argument...]
```

<br>

#### 예시로 사용할 test.txt

```
abc:def ghi
0123:456 789
```

<br>

#### 변수

| 변수명                         | 설명                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| $0                             | awk 프로그램이 실행 중 읽고 있는 현재 행을 의미합니다.       |
| $N (여기서, N은 임의의 자연수) | awk 프로그램의 입력으로 주어진 구분자로 현재 행을 잘랐을 때, 잘린 문자열 조각들 중 N번째 문자열 조각을 의미합니다. |
| length                         | 현재 행의 길이를 의미합니다.                                 |
| FS                             | **F**ield **S**eperator, 파일의 구분자입니다.                |
| NR                             | **N**umber of **R**ecords 레코드의 개수인데, 각 줄이 레코드이다. 결국 입력파일의 **줄 수**를 의미합니다. 정의 상, awk 프로그램 어느 명령문에서든 동일한 값을 반환합니다. |
| FNR                            | 현재 명령문이 실행 중인 입력파일의 **줄 수**를 의미합니다.   |
| NF                             | **N**umber of **F**ields, 즉 현재 행에서 구분자로 잘린 필드의 개수를 의미합니다. |
| RLENGTH                        | **match** 함수로 매칭된 문자열의 길이를 의미합니다.          |
| RSTART                         | **match** 함수로 매칭된 문자열의 시작 위치를 의미합니다. (행의 첫 번째 문자는 1을 의미합니다.) |

그 외 명령문은 [참고문서](https://man7.org/linux/man-pages/man1/awk.1p.html)를 확인하세요.

<br>

#### 명령문

명령문은 기본적으로 ISO C언어 표준문법을 기반으로 작성되어야 합니다.

| 명령문                                                 | 설명                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| print <출력할 변수>                                    | 주어진 인자를 출력합니다.                                    |
| if (<조건절>) { 수행문 }                               | 조건절에 맞으면, 수행문을 실행합니다.                        |
| for <변수명 in 배열> { 수행문 }                        | 배열에 들어있는 인자들을 하나씩 꺼내어 수행문을 반복합니다.<br />이 때, 현재 반복되는 인자를 가리키기 위해서는 변수명을 사용합니다. |
| match(<비교할 문자열>, /<정규표현식>/)                 | 비교할 문자열이 패턴과 일치하면, 패턴이 시작하는 위치를 반환합니다. (1부터 시작), 패턴이 일치하면, RSTART 값과 RLENGTH 값이 설정됩니다. |
| length(<문자열>)                                       | 문자열의 길이를 반환합니다.                                  |
| substr(<문자열>, 시작위치:m [, 추출할 문자열 길이: n]) | 주어진 문자열에서 시작위치 m(문자열의 첫번째 문자는 1로 계산)부터 n개의 문자열을 추출하여 반환합니다. |

그 외 명령문은 [참고문서](https://man7.org/linux/man-pages/man1/awk.1p.html)를 확인하세요.

<br>

#### 프로그램 구조 및 option

| option                      | 설명                                                         | 사용법 예시                                                  |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| awk 명령어 구조             | awk [option] '`awk 프로그램`' `입력파일`                     |                                                              |
| awk 프로그램 구조           | BEGIN { 명령문 }<br /></`매칭문자열 패턴`/> 또는 <조건문> {`명령문`}<br />END { 명령문 } | awk '/ab/' test.txt<br />test.txt 파일의 내용에서 문자열 `ab`가 포함된 행들을 출력합니다.<br />실행결과<br />abc:def ghi<br /> |
| BEGIN / END                 | BEGIN 뒤 명령문은 awk 프로그램 실행 전, 수행됩니다.<br />END 뒤 명령문은 awk 프로그램 종료 후, 수행됩니다.<br />일반 명령문은 조건문 또는 매칭문자열 패턴에 맞는 행을 읽은 뒤, 수행됩니다. 이 명령문이 생략되면, 기본적으로 해당 행을 출력합니다. | awk 'BEGIN { count = 0 } { count ++ } END {print count}' test.txt<br />일반 명령문이 실행되는 횟수를 출력합니다.<br />실행결과<br />2 |
| 조건문                      | 일반 명령문은 조건문 또는 매칭문자열 패턴에 맞는 행을 읽은 뒤, 수행됩니다. 이 명령문이 생략되면, 기본적으로 해당 행을 출력합니다. | awk 'BEGIN { count = 0 } NR == 1{ count ++ } END {print count}' test.txt<br />현재 실행 중인 **행의 번호**가 1일 때에만, count를 증가시킵니다. 최종적으로 count를 출력합니다.<br />실행결과<br />1 |
| 정규표현식                  | `'/매칭문자열/'`에서 단순히문자열을 기준으로 필터링할 수도 있지만, 정규표현식 형태로 필터링할 수도 있습니다.<br />패턴이 생략되면, 모든 행이 매칭됩니다. | awk '/[a-zA-Z]+/ {print $0}' test.txt<br />입력파일의 내용에서 하나의 알파벳이라도 포함되는 행을 출력합니다.<br />실행결과<br />abc:def ghi |
| 일반명령문을 여러 줄로 입력 | 일반 명령문 여러줄을 입력할 때는 각각의 명령문을 `;` 문자로 구분합니다. | awk '{len=0; for (i=1; i<=NF; i++) { print $i; len += length($i) }; print(len)}' test.txt<br />입력파일의 내용에서 각 행을 공백문자를 구분자로 하여 잘려진 문자열을 모두 출력하고, 해당 문자열의 길이를 더하여 출력합니다.<br />실행결과<br />abc:def<br/>ghi<br/>10<br/>0123:456<br/>789<br/>11 |
| -F '`구분문자`'             | 파일의 구분자로 사용할 문자를 지정합니다. <br />옵션 생략 시, 기본으로 사용되는 구분자는 공백문자입니다.<br />여러 개의 문자를 한번에 지정하는 것도 가능합니다. <br />`[]` 안에 있는 문자들은 모두 구분문자로 인식합니다. | awk -F '[: ]' '{print $1, $2, $3}' test.txt<br />입력파일의 내용에서 각 행을 물음표와 공백문자를 구분자하여 잘려진 문자열 1, 2, 3을 출력합니다.<br />실행결과<br />abc def ghi<br/>0123 456 789 |
| -f <프로그램 파일>          | 실행할 awk 프로그래밍을 명령줄에 명시하는 대신, 파일을 통해 명시할 때, 사용하는 옵션입니다. | 자주 사용하지 않는 옵션으로 추후 필요 시, 실습               |

<br>

### 상황에 따라서 사용하는 awk 명령어

#### 특정 시간 범위 내의 로그들만 콘솔에 출력

```shell
awk '"16:29:34" <= $1 && $1 <= "16:31:44"' catalina.out.2021-11-30
```

<br>

#### 특정 시간 범위 내의 로그들을 파일로 추출

```shell
awk '{ if ("17:31:43" <= $2 && $2 <= "17:33:20") print }' catalina.out.2021-11-23 > old_deploy.txt
awk '{ if ("17:59:47" <= $1 && $1 <= "18:01:51") print }' catalina.out.2021-11-30 > new_deploy.txt
```

<br>

#### 특정 시간 범위 내의 패턴 문자열을 가진 로그들을 파일로 추출

```shell
awk '/Exception/ { if ("17:31:43" <= $2 && $2 <= "17:33:20") print }' catalina.out.2021-11-23 > old_deploy.txt
awk '/Exception/ { if ("21:48:09" <= $2 && $2 <= "21:50:06") print }' catalina.out.2021-11-30 > new_deploy.txt
```

<br>

#### 배포시점이 다른 2개의 로그파일을 추출하고, 비교

```shell
awk '{ if ("17:31:43" <= $2 && $2 <= "17:33:20") print substr($0, index($0,$5)) }' catalina.out.2021-11-23 > old_deploy.txt


awk '{ if ("17:31:43" <= $2 && $2 <= "17:33:20") print substr($0, index($0,$4)) }' catalina.out.2021-11-23 > old_deploy.txt
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

위와 같이 작업하면, 로깅의 내용 중 여러줄에 걸쳐 출력되는 내용은 출력 값이 이상해지게 됩니다.

이를 피하기 위해서 1줄짜리 출력내용에만 위의 조건을 적용하는 코드로 수정합니다.

```
awk '{ 
	if ("17:31:43" <= $2 && $2 <= "17:33:20") 
	{
		if (length($4) == 5)
		{
			print substr($0, index($0,$4))
		}
		else
		{
			print
		}
	}
}' catalina.out.2021-11-23 > old_deploy.txt


awk '{ if ("17:59:47" <= $1 && $1 <= "18:01:51") print substr($0, index($0,$4)) }' catalina.out.2021-11-30 > new_deploy.txt
```

<br>

### awk 내장 함수

index와 substr 함수의 사용예시를 쉽게 파악하기 위해 다음 링크를 참고합니다.

https://www.geeksforgeeks.org/built-functions-awk/

<br>

### match 함수를 통한 패턴 검색 및 추출

각 줄에서 alphabet 문자만 추출하는 예시를 들겠습니다.

예제의 입력으로 주어질 input.txt의 내용은 아래와 같습니다.

```
!@#$
1234test5678
%^&*
```

<br>

아래 명령어를 수행합니다.

```shell
awk 'match($0, /[a-zA-Z]+/) {
	print substr($0, RSTART, RLENGTH)
}' input.txt

// 결과
test
```

위 명령어에서 `RSTART`는 매칭되는 문자의 시작 index를 의미하며, `RLENGTH`는 매칭되는 문자의 길이를 의미합니다.

<br>

### nginx access log에서 request path로 `/mobile/notice/view`를 포함하면서, response code가 500 이었을 때, remote ip를 중복 제거하여 추출

```
awk '/mobile\/notice\/view/ { if ($11 == 500) print $1}' /home/irteam/logs/nginx/nginx_access.log | sort | uniq
```

<br>

### 참고자료

https://man7.org/linux/man-pages/man1/awk.1p.html

https://ko.wikipedia.org/wiki/AWK

https://recipes4dev.tistory.com/171

https://www.poftut.com/awk-if-if-else-else-statement-or-conditional-statements/

https://stackoverflow.com/questions/13975493/log-file-search-based-on-date-and-time-range-on-multiple-log-files-using-awk?rq=1

