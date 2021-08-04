# PriorityQueue의 enhanced for문에 대한 오해

### 문제의 발생

백준 문제를 풀던 중 PriorityQueue (이하, PQ)를 이용하여 문제를 풀고 있었습니다.

PQ 에 담겨있는 원소들을 `우선순위순` 으로 뽑아내길 원하여 아래와 같이 하나씩 빼내는 연산을 하였습니다.

```java
PriorityQueue<Path> pathList = new PriorityQueue<>(new PathComparator());

... // pathList에 path 삽입

    // enhanced for문 사용
for (Path path: pathList) {
	// path 관련 연산
}
```

###### 하지만, 위와 같은 과정은 제가 원하는대로 `우선순위순` 으로 원소들을 추출해내지 못했습니다.

`우선순위순` 으로 원소들을 추출해내기 위해서는 아래와 같이 코드를 작성해야합니다.

```
PriorityQueue<Path> pathList = new PriorityQueue<>(new PathComparator());

... // pathList에 path 삽입

while (!pathList.isEmpty()) {
	Path path = pathList.poll();
	// path 관련 연산
}
```

<br>

### 문제의 원인분석

#### 의문점

저는 의문점이 들었습니다. 의문점이 드는 구간은 다음 2가지였습니다.

첫번째, 정렬이 완료된 `List 구현체`의 원소를 `enhanced for문` 으로 추출할 때는 정렬된 순서대로 원소를 추출할 수 있었습니다.

두번째, 첫번째에 근거하여 PQ 또한, 제가 지정한 순서대로 정렬이 되어있으니, 정렬된 순서대로 원소를 추출해야되는 것 아닐까?

<br>

#### 향상된 for문(enhanced for문)

이 문제를 해결하기 위해서, `enhanced for문` 이 내부적으로 어떻게 동작하는지 파악했습니다.

(참고: https://docs.oracle.com/javase/specs/jls/se7/html/jls-14.html#jls-14.14.2 )

`enhanced for문` 은 내부적으로 `Iterator` 인터페이스 구현이 존재하거나, `Array` 형이어야 합니다.

`enhanced for문`을 컴파일하면, 실제로는 아래의 예시처럼 변환됩니다.

##### Iterable 구현체의 향상된 for문 컴파일 시

```java
// java 파일
for(T element : collection) {
	System.out.println(element);
}

// class 파일
for(Iterator<T> it = collection.iterator(); it.hasNext(); ) {
	System.out.println((T) it.next());
}
```

<br>

##### Array 형의 향상된 for문 컴파일 시

```java
// java 파일
for(T element : array) {
	System.out.println(element);
}

// class 파일
for(T index = 0; index < array.length; index ++) {
	System.out.println(array[index]);
}
```

즉, `Iterable 구현체`를 `enhanced for문` 사용 시, 내부적으로 `Iterator` 를 이용합니다.

<br>

#### PriorityQueue의 Iterator

그렇다면, `Iterable 구현체`인 PQ의 `enhanced for문` 은 왜 순서를 보장하지 않을까요?

###### 이유는, PQ의 Iterator는 순서를 보장하지 않기 때문입니다.

![image-20210805013224775](./images/pq-misunderstanding-1)

(참고: https://docs.oracle.com/javase/7/docs/api/java/util/PriorityQueue.html )

<br>

### 추가 정리

그렇다면, 앞에서 언급한 `PQ외의 다른 자료구조`는 `enhanced-for문` 과 함께 쓰일 때의 정렬순서는 어떻게 될까요?

표로 정리하면, 아래와 같습니다.

| 자료구조  | 정렬 후, 향상된 for문과 사용 시, 순서보장여부 |
| --------- | --------------------------------------------- |
| Array 형  | O                                             |
| ArrayList | O                                             |
| HashSet   | X                                             |
| HashMap   | X                                             |
| TreeSet   | O                                             |
| TreeMap   | O                                             |

#### Set

Set 인터페이스도 기본적으로는 `enhanced for문` 과 함께 쓰이면, 정렬순서를 보장하지 않습니다. (아래 사진자료 참고)

![image-20210805015217699](./images/pq-misunderstanding-2)

#### TreeSet

그러나, TreeSet의 경우에는 Iterator 가 순서를 보장합니다. (아래 사진자료 참고)

![image-20210805015449968](./images/pq-misunderstanding-3)

#### TreeMap

TreeMap 도 TreeSet과 비슷하게 Iterator가 순서를 보장합니다.

![image-20210805015901175](./images/pq-misunderstanding-4)

<br>

### 참고자료

- https://stackoverflow.com/questions/8129122/how-to-iterate-over-a-priorityqueue

- https://docs.oracle.com/javase/specs/jls/se7/html/jls-14.html#jls-14.14.2
- https://jcp.org/aboutJava/communityprocess/jsr/tiger/enhanced-for.html

- https://docs.oracle.com/javase/7/docs/api/java/util/PriorityQueue.html 

- https://docs.oracle.com/javase/7/docs/api/java/util/Set.html
- https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html
- https://docs.oracle.com/javase/7/docs/api/java/util/TreeMap.html

