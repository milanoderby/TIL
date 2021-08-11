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

