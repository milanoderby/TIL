# substring 명령어

bash 에서는 다양한 방법으로 문자열을 추출할 수 있습니다.

### 1. 환경변수의 확장문법을 사용하는 방법

#### 기본 문법

`${환경변수명:시작index:추출할 길이}`

여기서, index는 0부터 시작합니다.

<br>

#### 생략 문법

`추출할길이` 를 생략하면, 시작 index부터 문자열의 끝까지 추출합니다.

`${환경변수명:시작index}`

<br>

```shell
STR="0123Linux9"
echo ${STR:4:5}
echo ${STR:4}

// 결과
Linux
Linux9
```

<br>

### 2. cut 명령어

#### 기본 문법

input으로 들어온 문자열에 대해 추출연산을 수행합니다.

`cut -c <추출할 범위>`

`cut -c index`

여기서 index는 1부터 시작합니다.

<br>

`cut -c index1,index2`

위치가 다른 2개의 문자를 순서대로 추출하려면 위와 같이 사용합니다.

<br>

`cut -c index1-index2`

연속된 문자열을 추출하려면, 위와 같이 사용합니다.

<br>

```shell
#!/bin/bash

STR="0123Linux9"

echo $STR | cut -c 5
echo $STR | cut -c 5,9
echo $STR | cut -c 5-9
cut -c 5-9 <<< $STR

// 결과
L
Lx
Linux
Linux
```

<br>

### 참고자료

https://www.baeldung.com/linux/bash-substring