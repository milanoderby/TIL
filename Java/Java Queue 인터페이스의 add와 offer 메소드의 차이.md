# Java Queue 인터페이스의 add와 offer 메소드의 차이

### 개요

Java 의 Queue 인터페이스에는 `추가` 연산을 수행하는 2가지 메소드가 있습니다.

단순 추가 작업에 대해서 큰 차이가 없어보이는 이 2가지 메소드에는 아주 미묘한 차이점이 있습니다.

Queue 인터페이스의 명세에서 이를 확인할 수 있습니다.

<br>

### Queue 인터페이스 명세

```java
public interface Queue<E> extends Collection<E> {
    /**
     * Inserts the specified element into this queue if it is possible to do so
     * immediately without violating capacity restrictions, returning
     * {@code true} upon success and throwing an {@code IllegalStateException}
     * if no space is currently available.
     *
     * @param e the element to add
     * @return {@code true} (as specified by {@link Collection#add})
     * @throws IllegalStateException if the element cannot be added at this
     *         time due to capacity restrictions
     * @throws ClassCastException if the class of the specified element
     *         prevents it from being added to this queue
     * @throws NullPointerException if the specified element is null and
     *         this queue does not permit null elements
     * @throws IllegalArgumentException if some property of this element
     *         prevents it from being added to this queue
     */
    boolean add(E e);

    /**
     * Inserts the specified element into this queue if it is possible to do
     * so immediately without violating capacity restrictions.
     * When using a capacity-restricted queue, this method is generally
     * preferable to {@link #add}, which can fail to insert an element only
     * by throwing an exception.
     *
     * @param e the element to add
     * @return {@code true} if the element was added to this queue, else
     *         {@code false}
     * @throws ClassCastException if the class of the specified element
     *         prevents it from being added to this queue
     * @throws NullPointerException if the specified element is null and
     *         this queue does not permit null elements
     * @throws IllegalArgumentException if some property of this element
     *         prevents it from being added to this queue
     */
    boolean offer(E e);
    
    /**
     * 이하 생략
     */
}
```

<br>

### add(E)와 offer(E)의 차이

#### 예외 및 반환 값

**add(E)** 메소드의 설명부를 잘 살펴보면, Queue에 할당된 공간이 부족할 때, add 메소드를 수행하면 이 작업은 실패하고, **IllegalStateException 예외**를 던진다는 것을 알 수 있습니다.

반면, **offer(E)** 메소드의 설명부를 살펴보면, Queue에 할당된 공간이 부족할 때, offer 메소드를 수행하면 이 작업이 실패하는 것은 같지만, 예외를 발생시키지 않는다는 것을 알 수 있습니다. 그래서 예외를 발생시키는 방식의 **add메소드**를 사용하는 것을 권합니다. 대신, 공간 부족으로 인한 추가 연산이 실패할 때, return 값으로 **false**를 반환하는 것을 알 수 있습니다.

<br>

#### 사용

즉, 공간 부족으로 인한 추가연산 실패 시, 처리방식을 예외를 던지는 방식으로 할 것인지 return 값으로 실패를 감지하는 방식으로 할 것인지에 따라 2가지 메소드 중 하나를 선택해서 사용하면 됩니다.

