# Java Deque (ArrayDeque vs LinkedList)

### 정의

Java Collections Framework에는 에는 Deque 인터페이스가 있습니다.

Queue는 뒤에서 요소를 추가/획득하고, 앞에서 제거/획득하는 방식의 자료구조라면, 

Deque는 앞, 뒤 양쪽에서 요소를 추가/제거 및 획득할 수 있는 자료구조입니다.

그리고, 이 인터페이스를 구현하는 2가지 구현체가 존재합니다.

영어발음은 `Deck` 이라고 합니다.

<br>

### ArrayDeque

ArrayDeque는 말 그대로 **크기 조절이 가능한 배열을 이용**하여 Deque 인터페이스를 구현한 구현체입니다. 실제 구현 방식이 양쪽 끝에 배열 인덱스를 두고, 

요소로 **Null 값을 추가할 수 없습니다.**

ArrqyDeque는 **양쪽에서 추가/삭제 연산 시, LinkedList보다 더 효율적**입니다. (Oracle 공식문서에는 그 이유가 나와있지 않지만, 그 이유는 다음으로 추정됩니다. ArrayDeque는 배열과 인덱스를 이용하기 때문에 이미 할당된 공간을 사용하는 반면, LinkedList는 요소 생성 시, 새로운 Node 객체를 힙영역에 생성(메모리 할당작업)하고, 요소 제거 시, GC가 돌면서 메모리 반환작업이 이루어지기 때문입니다.)

**LinkedList보다 메모리를 적게 사용**합니다. LinkedList는 다음 요소를 가리키는 객체와 요소의 값이라는 두 가지 값을 저장하기 위해 Node 객체를 내부적으로 생성하기 때문입니다.

<br>

### LinkedList

LinkedList는 이름을 통해 알 수 있듯, Deque 인터페이스를 **List로 구현**한 구현체입니다.

요소로 **Null 값을 추가할 수 있습니다.**

LinkedList는 **반복문 수행 중 요소 삭제 시, ArrayDeque보다 더 효율적**입니다. ArrayDeque는 배열을 이용하기 때문에 중간에 삭제된 공간을 복사연산으로 채워야하기 때문에 O(N)의 시간복잡도를 가지는 반면, LinkedList는 중간에 삭제된 Node를 제거하고, 삭제된 Node의 앞과 뒤 객체를 서로 참조하게하면 됩니다.

<br>

### 동시성 제어

ArrayList와 LinkedList 모두 멀티스레드에 의한 동시접근을 지원하지 않습니다. 즉, 동시성을 제어하지 않는 자료구조입니다.

**동시성 제어까지 구현한 Deque 구현체는 LinkedBlockingDeque**입니다.

<br>

### 참고자료

https://docs.oracle.com/javase/tutorial/collections/implementations/deque.html