# Arrays 클래스

배열을 조작하는 여러가지 메소드(정렬이나, 탐색 등)를 제공하는 클래스입니다.

<br>

### asList(T... a) 메소드

데이터타입이 T인 인자들()을 받아 List\<T> 형태로 반환하는 메소드입니다.

###### 이 때, (T...) 은 데이터타입이 T인 인자의 개수는 정해지지 않고 계속 받을 수 있음을 의미합니다.

```java
public static void main(String[] args) {

    List<Integer> numList = Arrays.asList(1,2,3);
    for (Integer num : numList) {
        System.out.print(num + " ");
    }
}

// 출력결과
1 2 3
```

<br>

### copyOf(T[] array, int newLength) 메소드

데이터타입이 T인 array를 복사하여 새로 배열을 생성합니다.

###### 이 때, 인자로 주어진 newLength 길이 만큼만 배열을 복사합니다.

<br>

newLength가 기존 배열의 크기보다 크다면, 기존 배열보다 늘어나는 부분에 해당하는 배열의 값은 0으로 채웁니다.

newLength가 기존 배열의 크기보다 작다면, 기존 배열에서 newLength의 크기만큼만 값을 복사합니다.

<br>

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] sortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
        sortedArray[i] = i;
    }

    int[] copiedArray1 = Arrays.copyOf(sortedArray, originalLength);
    for (int i = 0; i < copiedArray1.length; i++) {
        System.out.print(copiedArray1[i] + " ");
    }
    System.out.println();

    int[] copiedArray2 = Arrays.copyOf(sortedArray, originalLength - 5);
    for (int i = 0; i < copiedArray2.length; i++) {
        System.out.print(copiedArray2[i] + " ");
    }
    System.out.println();

    int[] copiedArray3 = Arrays.copyOf(sortedArray, originalLength + 5);
    for (int i = 0; i < copiedArray3.length; i++) {
        System.out.print(copiedArray3[i] + " ");
    }
    System.out.println();
}

// 출력결과
0 1 2 3 4 5 6 7 8 9 10 
0 1 2 3 4 5 
0 1 2 3 4 5 6 7 8 9 10 0 0 0 0 0 
```

<br>

### copyOfRange(T[] array, int from, int to) 메소드

데이터타입이 T인 array를 복사하여 새로 배열을 생성합니다.

###### 이 때, 인자로 주어진 `from` 인덱스부터, `to - 1` 인덱스까지의 배열을 복사합니다.

<br>

`to`값이 기존 배열의 크기보다 크다면, 기존 배열보다 늘어나는 부분에 해당하는 배열의 값은 0으로 채웁니다.

###### (기존 배열의 `to - 1` 인덱스의 값을 접근하는 것이라 오류가 난다고 생각할 수 있지만, 그렇지 않습니다.

<br>

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] sortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
        sortedArray[i] = i;
    }

    int[] copiedArray1 = Arrays.copyOfRange(sortedArray, 3, originalLength);
    for (int i = 0; i < copiedArray1.length; i++) {
        System.out.print(copiedArray1[i] + " ");
    }
    System.out.println();

    int[] copiedArray2 = Arrays.copyOfRange(sortedArray, 3, originalLength + 2);
    for (int i = 0; i < copiedArray2.length; i++) {
        System.out.print(copiedArray2[i] + " ");
    }
    System.out.println();
}

// 출력결과
3 4 5 6 7 8 9 10 
3 4 5 6 7 8 9 10 0 0 
```

<br>

### fill(T[] array, int val) 메소드

데이터타입이 T인 array 배열의 값을 val로 채웁니다.

###### 이 때, 기존 array 배열의 할당된 크기만큼 모든 값이 val로 채워집니다.

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] sortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
    	sortedArray[i] = i;
    }

	Arrays.fill(sortedArray, 5);
	for (int i = 0; i < originalLength; i++) {
		System.out.print(sortedArray[i] + " ");
	}
	System.out.println();
}

// 출력결과
5 5 5 5 5 5 5 5 5 5 5 
```

<br>

### fill(T[] array, int from, int to, int val) 메소드

데이터타입이 T인 array 배열의 값을 val로 채웁니다.

###### 이 때, 인자로 주어진 `from` 인덱스부터, `to - 1` 인덱스까지의 배열을 복사합니다. 내부적으로는 아래와 같이 구현되어 있기 때문입니다.

```java
public static void fill(int[] a, int fromIndex, int toIndex, int val) {
	rangeCheck(a.length, fromIndex, toIndex);
	for (int i = fromIndex; i < toIndex; i++)
		a[i] = val;
}
```

<br>

#### 예시

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] sortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
    	sortedArray[i] = i;
    }

	Arrays.fill(sortedArray, 1, 2, 99999);
    for (int i = 0; i < originalLength; i++) {
        System.out.print(sortedArray[i] + " ");
    }
    System.out.println();
}

// 출력결과
0 99999 2 3 4 5 6 7 8 9 10 
```

<br>

### sort(T[] array) 메소드

인자로 전달된 데이터타입이 T인 배열 array를 정렬합니다.

###### 데이터타입이 `Primitive Type`인 경우, 디폴트 정렬방식인 `Natural Order` 방식으로 정렬됩니다.

이는 대부분의 `Primitive Type` 에 대해 오름차순입니다.

###### 반면, T가 `Primitive Type` 이 아닌 데이터의 경우, 데이터 T의 정렬순서를 명시하기 위해 Comparator\<T> 객체를 함께 전달해줘야 합니다.

<br>

###### 내부적으로 2개의 피봇을 사용하는 퀵소트로 구현되어 있습니다. Collections.sort와 내부 구현방식이 다릅니다.

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] unsortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
        unsortedArray[i] = originalLength - i;
        System.out.print(unsortedArray[i] + " ");
    }
    System.out.println();

    Arrays.sort(unsortedArray);
    for (int i = 0; i < originalLength; i++) {
        System.out.print(unsortedArray[i] + " ");
    }
    System.out.println();
}

// 출력결과
11 10 9 8 7 6 5 4 3 2 1 
1 2 3 4 5 6 7 8 9 10 11 
```

<br>

### sort(T[] array, int from, int to) 메소드

인자로 전달된 데이터타입이 T인 배열 array를 정렬합니다.

###### 이 때, 인자로 주어진 `from` 인덱스부터, `to - 1` 인덱스까지의 배열을 정렬합니다.

```java
public static void main(String[] args) {
	int originalLength = 11;
    int[] unsortedArray = new int[originalLength];
    for (int i = 0; i < originalLength; i++) {
        unsortedArray[i] = originalLength - i;
        System.out.print(unsortedArray[i] + " ");
    }
    System.out.println();

    Arrays.sort(unsortedArray, 1, 3);
    for (int i = 0; i < originalLength; i++) {
        System.out.print(unsortedArray[i] + " ");
    }
    System.out.println();
}

// 출력결과
11 10 9 8 7 6 5 4 3 2 1 
11 9 10 8 7 6 5 4 3 2 1
```

위에서 설명한대로 예시의 결과 역시, `unsortedArray[1] ~ unsortedArray[3 - 1]` 까지의 배열만 정렬이 되었습니다.

<br>

### binarySearch(T[] array, int key) 메소드

인자로 전달된 데이터타입이 T인 `정렬된` 배열 array에서 key값을 탐색 후, 찾았을 때, `index`를 반환합니다.

###### 이 때, array 안에 key 값이 없다면, `key` 가 있을만한 `상대적 위치값`을 반환합니다.

```java
public static void main(String[] args) {
	int originalLength = 5;
    int[] sortedArray = new int[originalLength];
    System.out.println("예시로 사용할 정렬된 배열");
    for (int i = 0; i < originalLength; i++) {
        sortedArray[i] = 2 * i;
        System.out.print(sortedArray[i] + " ");
    }
    System.out.println();

    System.out.println("key를 찾았을 때, 반환되는 index 값");
    for (int key = 0; key < 2 * originalLength; key += 2) {
        System.out.println("key: " + key + ", index: " + Arrays.binarySearch(sortedArray, key) + " ");
    }
    System.out.println();

    System.out.println("key를 못찾았을 때, 반환되는 index 값");
    for (int key = -1; key < 2 * originalLength; key += 2) {
        System.out.println("key: " + key + ", index: " + Arrays.binarySearch(sortedArray, key) + " ");
    }
    System.out.println();
}

// 출력결과
예시로 사용할 정렬된 배열
0 2 4 6 8 
key를 찾았을 때, 반환되는 index 값
key: 0, index: 0 
key: 2, index: 1 
key: 4, index: 2 
key: 6, index: 3 
key: 8, index: 4 

key를 못찾았을 때, 반환되는 index 값
key: -1, index: -1 
key: 1, index: -2 
key: 3, index: -3 
key: 5, index: -4 
key: 7, index: -5 
key: 9, index: -6 
```

<br>

### binarySearch(T[] array, int from, int to, int key) 메소드

인자로 전달된 데이터타입이 T인 `정렬된` 배열 array에서 key값을 탐색 후, 찾았을 때, `index`를 반환합니다.

###### 이 때, 인자로 주어진 `from` 인덱스부터, `to - 1` 인덱스까지 배열 범위에서 key 값을 탐색합니다.

###### 이 때, array 안에 key 값이 없다면, `key` 가 있을만한 `상대적 위치값`을 반환합니다. 위의 메소드와는 다르게 전체 배열의 범위가 아닌 배열의 일부 범위에서 탐색하므로 key값이 같더라도, from, to 값이 다르면, 다른 값을 반환합니다. 

###### 즉, 이 메소드에서는 key 값이 없을 때, 상대적 위치값 결과를 이해하는 것은 쉽지 않으므로 이 결과를 활용하는 생각은 안하는 것이 좋습니다.

```java
public static void main(String[] args) {
	int originalLength = 5;
    int[] sortedArray = new int[originalLength];
    System.out.println("예시로 사용할 정렬된 배열");
    for (int i = 0; i < originalLength; i++) {
        sortedArray[i] = 2 * i;
        System.out.print(sortedArray[i] + " ");
    }
    System.out.println();

    System.out.println("key를 찾았을 때, 반환되는 index 값");
    System.out.println("from: 1, to: 2, key: 2, index: " + Arrays.binarySearch(sortedArray, 1, 2, 2) + " ");
    System.out.println();

    System.out.println("key를 못찾았을 때, 반환되는 index 값");
    System.out.println("from: 1, to: 2, key: -1, index: " + Arrays.binarySearch(sortedArray, 1, 2, -1) + " ");
}

// 출력결과
예시로 사용할 정렬된 배열
0 2 4 6 8 
key를 찾았을 때, 반환되는 index 값
from: 1, to: 2, key: 2, index: 1 

key를 못찾았을 때, 반환되는 index 값
from: 1, to: 2, key: -1, index: -2 
```

<br>
