# Java Integer형 ArrayList를 int형 배열로 변환방법

### 일반적인 방법

```java
public static int[] solution() {
	List<Integer> integerList = new ArrayList<>();

	int[] intArray = new int[integerList.size()];
	for (int i = 0; i < intArray.length; i++) {
		intArray[i] = integerList.get(i);
	}
	return intArray;
}
```

<br>

### stream 연산 이용하는 방법

```java
public static int[] solution() {
	List<Integer> integerList = new ArrayList<>();
	int[] intArray = integerList.stream().mapToInt(Integer::intValue).toArray();
	return intArray;
}
```

<br>

### 주의할 점

List에는 toArray메소드를 지원하는데, 이는 List의 자료구조를 편하게 배열로 변하게 해주는 메소드입니다.

허나, 현재, List의 Generic Type은 Integer이고, 배열의 데이터는 int 타입이기 때문에 바로 변환할 수가 없습니다.

따라서, 위와 같이 `stream().mapToInt(Integer::intValue)` 연산과정을 거친 다음에 toArray를 사용할 수 있습니다.

<br>

### toArray 메소드

그렇다면, 실제 toArray를 사용할 수 있는 경우에 어떤 방식으로 사용되는지 확인해보자.

```java
List<String> stringList = Arrays.asList("String1", "String2", "String3");
String[] stringArray = stringList.toArray(new String[0]);
for (String s : stringArray) {
	System.out.println(s);
}
```

<br>

여기서, String 배열의 인자로 주어지는 0은 배열의 크기를 0으로 할당하지만, 이는 중요하지 않습니다.

toArray 메소드의 인자로 주어지는 배열객체의 size는 아래와 같이 동작하기 때문입니다.

1. List를 toArray 메서드에 파라메터로 넘어가는 배열 객체의 size만큼의 배열로 전환합니다.
2. 단, 해당 List size가 인자로 넘어가는 **배열 객체의 size보다 클때, 해당 List의 size로 배열이 만들어집니다.**
3. 반대로 해당 List size가 인자로 넘어가는 **배열객체의 size보다 작을때는, 인자로 넘어가는 배열객체의 size로 배열이 만들어진다.**
