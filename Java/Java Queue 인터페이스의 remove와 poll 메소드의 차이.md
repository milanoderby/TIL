# Java Queue 인터페이스의 remove와 poll 메소드의 차이

### 개요

Java 의 Queue 인터페이스에는 `삭제` 연산을 수행하는 2가지 메소드가 있습니다.

단순 삭제 작업에 대해서 큰 차이가 없어보이는 이 2가지 메소드에는 아주 미묘한 차이점이 있습니다.

Queue 인터페이스의 명세에서 이를 확인할 수 있습니다.

<br>

### Queue 인터페이스 명세

```java
public interface Queue<E> extends Collection<E> {
    
    /**
     * Retrieves and removes the head of this queue.  This method differs
     * from {@link #poll poll} only in that it throws an exception if this
     * queue is empty.
     *
     * @return the head of this queue
     * @throws NoSuchElementException if this queue is empty
     */
    E remove();

    /**
     * Retrieves and removes the head of this queue,
     * or returns {@code null} if this queue is empty.
     *
     * @return the head of this queue, or {@code null} if this queue is empty
     */
    E poll();
    
    /**
     * 이하 생략
     */
}
```

<br>

### remove(E)와 poll(E)의 차이

#### 예외 및 반환 값

**remove(E)** 메소드의 설명부를 살펴보면, Queue에서 삭제할 요소가 없을 때, remove 메소드를 수행하면 이 작업은 실패하고, **NoSuchElementException 예외**를 던진다는 것을 알 수 있습니다.

반면, **poll(E)** 메소드의 설명부를 살펴보면, Queue에서 삭제할 요소가 없을 때, poll 메소드를 수행하면 이 작업이 실패하는 것은 같지만, 예외를 발생시키지 않는다는 것을 알 수 있습니다. 대신, **null**을 반환하는 것을 알 수 있습니다.

<br>

#### 사용

즉, Queue가 비어있음에 따른 삭제연산 실패 시, 처리방식을 예외를 던지는 방식으로 할 것인지 return 값으로 실패를 감지하는 방식으로 할 것인지에 따라 2가지 메소드 중 하나를 선택해서 사용하면 됩니다.