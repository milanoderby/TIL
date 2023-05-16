# sort, uniq 명령어를 이용한 중복 제거

### 사용법

sort [중복을 제거할 파일명] | uniq

줄 단위로 정렬을 수행합니다.

동일한 내용을 가지는 줄이 여러 줄 있을 경우, 하나의 줄만 보이도록 중복을 제거합니다.

<br>

#### input.txt

```
bat
abc
apple
Abc
BALL
ABc
bat
```

<br>

#### 중복 제거 후, 파일 내용

`sort input.txt | uniq > output.txt`

`cat output.txt`

```
abc
Abc
ABc
apple
BALL
bat
```