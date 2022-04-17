# for 문

### 기본형 - script 외 shell 에서 바로 사용가능

```shell
for <변수명> in <반복할 리스트>; do <명령어> $<변수명>; done
```

<br>

### 리스트 생성방법

위에서 `<반복할 리스트>`를 만드는 방법에는 여러가지가 있습니다.

#### 숫자 리스트 생성방법

`{시작번호..끝번호}` 형식으로 사용합니다.

```shell
for num in {1..10}; do echo ${num}; done

// 실행결과
1
2
3
4
5
6
7
8
9
10
```

<br>

#### 문자 리스트 생성방법

`{one,two,...,last}` 형식으로 순서대로 나열합니다.

```shell
for num in {one,two,three}; do echo ${num}; done

// 실행결과
one
two
three
```

**나열하는 문자열과 `,` 사이에 공백이 있으면 정상적으로 출력되지 않으니 주의하세요.**

<br>

#### 다른 파일에 저장되어 있는 내용을 목록으로 가져오기

```shell
for var in $(cat test.txt); do echo ${var}; done

// 실행결과
test1
test2
test3
```

<br>

### 생략가능한 문법

1. 변수를 사용할 때, `${num}` 을 사용해도 되지만, `$num`으로 사용해도 됩니다.

2. 세미콜론(`;`) 대신, 개행을 한다면, 세미콜론은 필요하지 않습니다. 예시는 아래와 같습니다.

   ```shell
   for num in {1..10}
   do
   	echo $num
   done
   ```

<br>

### 중첩된 for문

```shell
for num1 in {2..4}; do
	for num2 in {2..4}; do
		result=$((num1*num2))
		echo "$num1 * $num2 = $result"
	done
done
```

<br>

### 참고자료

https://www.redhat.com/sysadmin/bash-scripting-loops