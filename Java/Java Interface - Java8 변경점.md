# Java Interface - Java8 변경점

### 기존

Java 8에서 interface에 대한 정의가 변경되었습니다.

기존에는 `public` 메소드만 선언가능했습니다. 또한, 이는 완전히 구현할 수 없는 추상메소드이며, 메소드의 input과 output만 정의하는 역할에 그쳤습니다.

<br>

### 변경사항

Java8 에서는 `default` 메소드와 `static` 메소드가 추가되었습니다.

#### default 메소드

default 메소드는 인터페이스 내에서 구현이 가능하며, 이를 구현한 객체에서 default메소드를 사용할 수 있습니다.

`Collection` 인터페이스의 `stream`, `parallelStream`, `spliterator` 메소드가 이런 default 메소드의 대표적인 예시입니다.

```java
    /**
     * Returns a sequential {@code Stream} with this collection as its source.
     *
     * <p>This method should be overridden when the {@link #spliterator()}
     * method cannot return a spliterator that is {@code IMMUTABLE},
     * {@code CONCURRENT}, or <em>late-binding</em>. (See {@link #spliterator()}
     * for details.)
     *
     * @implSpec
     * The default implementation creates a sequential {@code Stream} from the
     * collection's {@code Spliterator}.
     *
     * @return a sequential {@code Stream} over the elements in this collection
     * @since 1.8
     */
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }
```

<br>

의미: 인터페이스에서 `public` 메소드를 추가될 때, 구현체에서는 이를 모두 구현해야하는 문제점이 있습니다. 이런 불편한 점을 없애기 위해 `default` 메소드를 추가하였습니다.

<br>

### static 메소드

Class의 static 메소드와 마찬가지로, `Interface명.메소드명` 형식으로 호출해야합니다.

의미: Interface를 이용해서 간단한 기능을 가지는 유틸리티 메소드를 만들 수 있게 되었습니다.

<br>

### 참고자료

https://programmers.co.kr/learn/courses/5/lessons/241#