# EOL(End Of Line)이 어느 부분인지 모를 때, 입력받는 법

`EOL(End Of Line)`이 어느 부분인지 모를 때, 즉, 입력이 언제 끝날지 모르는 문제에서 입력을 받는 방법을 검색해보았습니다.

<br>

예를 들어, 문제의 입력조건이 다음과 같다고 하겠습니다. (참고: https://www.acmicpc.net/problem/5670)

###### 입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스의 첫째 줄에 사전에 속한 단어의 개수 N이 주어지며(1 ≤ N ≤ 105), 이어서 N개의 줄에 1~80글자인 영어 소문자로만 이루어진 단어가 하나씩 주어진다.

실제 입력은 다음과 같이 들어옵니다.

```
4
hello
hell
heaven
goodbye
3
hi
he
h
7
structure
structures
ride
riders
stress
solstice
ridiculous
```

<br>

이러한 문제의 입력을 받기 위해서 `Java의 2가지 입력메소드`로 나누어서 생각하겠습니다.

### 1. 입력 메소드로 Scanner 객체.next~() 를 사용할 때

```java
public static void main(String[] args) throws IOException {
	Scanner scanner = new Scanner(System.in);
	while (scanner.hasNextInt()) {
	int N = scanner.nextInt();
		for (int i = 0; i < N; i++) {
			String word = scanner.next();
		}
	}
}
```

<br>

### 2. 입력 메소드로 BufferedReader 객체.readLine() 를 사용할 때

입출력 시간을 줄이기 위해 버퍼 개념을 사용하는 `BufferedReader` 를 선호합니다.

```java
public static void main(String[] args) throws IOException {
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    
    String input;
	while ((input = br.readLine()) != null) {
	int N = Integer.parseInt(input);
		for (int i = 0; i < N; i++) {
			String word = br.readLine();
		}
	}
}
```