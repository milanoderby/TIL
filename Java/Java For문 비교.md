# Java For문 비교

## 기존 for문 vs 향상된 for문 (enhanced for loop statement)

### 기존 for문

``` java
for(int i=0; i<list.size(); i++){
    list.get(i).doSomething();
}
```

list의 인자들을 수정할 수 있습니다.

* 반복문을 돌면서, list에 새로운 인자들을 추가, 삭제가능합니다.

`index` 값을 알고 있습니다.

* index를 통해서, 같은 사이즈의 다른 배열을 참조 및 비교할 수 있습니다.
* index를 통해서, 현재 인자의 앞과 뒤 인자를 참조할 수 있습니다.

`Array List`와 같은 `Random Access` list에서 효율적이다.

* 반대로, `Linked List`에서는 `get(i)`를 구하기 위해 `head`와 `tail`로 부터 접근하기 때문에 `O(n)` 시간 복잡도를 가지므로, 위의 코드는 총 `O(n^2)` 시간복잡도를 가지게 됩니다.

`list.get(i)`와 같이 인자 값에 접근하는 메소드를 직접 명시해줘야하므로 사용성이 떨어집니다.

* 인자 값을 저장하는 `Collection`이 달라질 경우, 인자 값에 접근하는 메소드를 수정해야합니다.

구문 사용 중 에러가 발생하기 쉬운 요소가 있습니다.

* `int i = 0` 대신, `i = 0` 을 사용할 경우, 이전에 선언된 변수를 참조하므로 오류가 날 수 있습니다.
* `<` 대신, `>` 을 사용할 경우, 반복문이 실행되지 않습니다.
* `i++` 대신, `j++`을 사용할 경우, 무한루프가 실행됩니다.
* `.get(i)` 대신, `.get(j)`를 사용할 경우, 항상 같은 인자를 참조하게 됩니다.

int 변수 `index`를 `스택`에 생성합니다.

* 비용이 적게 듭니다.

<br>

### 향상된 For문 (java 5 부터 도입)

``` java
for(Example example : list) {
    example.doSomething();
}
```

list의 인자들을 수정할 수 없습니다.

* 인자를 수정할 때, `ConcurrentModificationException`이 발생합니다.

`index` 값을 모릅니다.

* index를 통해서, 같은 사이즈의 다른 배열을 참조 및 비교할 수 없습니다.
* index를 통해서, 현재 인자의 앞과 뒤 인자를 참조할 수 없습니다.

내부 `Iterator`를 사용하므로 모든 `Collection`에서 항상 효율적인 방식으로 작동합니다.

* `Linked List`의 경우에서도, 위의 코드는 `O(n)` 시간복잡도를 가집니다.

`Collection` 뿐만 아니라, `Iterable` 인터페이스를 구현한 클래스에서도 사용가능한 구문입니다.

* 인자 값을 저장하는 `Collection`이 달라져도, 인자 값에 접근하는 메소드를 수정할 필요가 없으므로 인자 값을 저장하는 자료구조를 `List` 에서 `Set`으로 또는 `Iterable interface를 구현한 Class`로 쉽게 바꿀 수 있습니다.

구문 사용이 쉽다.

* 더 적은 코드, 반복문을 실행하기 위해 별도의 변수 및 메소드(`index`, `get(i)`)는 필요하지 않습니다.

`Iterator` 를 구현한 자료구조를 사용할 경우, 3개의 int 변수가 `힙 영역`에 생성됩니다.

``` java
private class Itr implements Iterator<E> {
    int cursor;
    int lastRet = -1;
    int expectedModCount = modCount;
    // ...
```

* 비용이 많이 듭니다.

<br>

### 기존 for문 vs 향상된 for문 요약

|                             | 기존 for문                                                   | 향상된 for문                                                 |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 반복문 실행 중 인자 값 수정 | 가능                                                         | 불가, `ConcurrentModificationException`을 던집니다.          |
| 사용성, 가독성              | index라는 외부 Iterator를 사용하며, List.get(index)와 같이 인자 값에 접근하는 메소드를 직접 명시해줘야하므로 사용성이 떨어집니다. | 내부 Iterator를 사용하므로 구문 자체가 직관적이고, 가독성이 좋습니다. |
| 메모리                      | 비교적 비용이 싼 연산                                        | 비교적 비용이 비싼 연산                                      |

<br>

## 향상된 for문 vs Iterable.forEach()

### 향상된 For문 (java 5 부터 도입)

``` java
for(Example example : list){
    example.doSomething();
}
```

함수형 표현식 구문이 아닙니다.

* 내부 함수와 외부 함수의 결합도가 비교적 높습니다. 즉, 함수 내부 구문이 감싸는 반복문과 제어문에 영향을 줄 수 있습니다.
* break 문, continue문을 사용하여 반복문을 제어할 수 있습니다.
* `Checked Exception` 을 처리할 수 있습니다.

병렬처리를 지원하지 않습니다.

<br>

### Iterable.forEach() (java 8 부터 도입)

``` java
list.forEach(example -> example.doSomething());
```

함수형 표현식을 사용하기 위한 구문이다. 즉, 아래와 같이 의도된 코드입니다.

``` java
list.forEach(new Consumer<T>() {
    @Override
    public void accept(T example) {
        example.doSomething();
    }
});
```

내부 함수와 외부 함수의 결합도가 비교적 낮습니다.

* forEach문 안에 새로운 함수를 작성한 형태입니다. 즉, forEach 문 안에 구문은 반복문과 별개로 작동한다. 즉, 함수 내부 구문이 감싸는 반복문과 제어문에 영향을 줄 수 없습니다. 
* forEach 반복문 안에서 continue문의 역할은 return 문으로 할 수 있지만, break문의 역할은 할 수 없기 때문에, 반복문을 중간에서 멈출 수 없습니다.
* `Consumer`와 같은 일반적인 함수 인터페이스에서는 `Checked Exception`을 처리할 수 없습니다.

* 따라서, Checked Exception을 던지는 코드를`try-catch` 문과 `Throwable.propagate()`으로 감싸줘야 합니다. 하지만, 이 또한 forEach()문 안에서 뱉어지지 않고, 먹힐 수도 있습니다.

`parallelStream()`을 통해 병렬처리를 지원합니다.

* 일반적인 `stream()`보다 속도는 더 빠를 수 있으나, `stream` 형태로 변환하는 자체 오버헤드가 존재하여 `향상된 for문` 보다 빨라지지 않습니다.
* 메모리 관점이나, 동기화 관점에서 고려해야할 점이 늘어납니다.

<br>

### 그 외 여러 관점에서 바라본 `Iterable.forEach`의 장단점

유지보수성

* 기존 for문 방식과 혼용하면, 2가지 방식의 코드를 모두 이해해야되므로 가독성 및 유지보수성에 부작용이 있습니다.
* `함수 인터페이스`를 사용함으로서 `Strategy Pattern`을 이용해 다른 구현으로 실행되게 할 수 있습니다.

디버깅

* 함수 인터페이스라는 하나의 계층이 하나 더 생김으로써, 디버깅과정을 더 어렵게 만듭니다.
* 디버깅의 `step-through` 가 원하던대로 작동하지 않을 수 있습니다.
* 메인 반복문과 분리되기 때문에 메인 반복문을 더럽히지 않고, 함수 인터페이스 내에서 디버깅을 더 쉽게 구현할 수 있습니다.

<br>

## Iterable.forEach() vs Collections.stream().forEach()

변수 선언문

``` java
List<String> list = Arrays.asList("A", "B", "C", "D");
```

함수 인터페이스 선언문

``` java
Consumer<String> consumer = s -> { System.out::println };
```

<br>

### Iterable.forEach() (java 8 부터 도입)

``` java
list.forEach(consumer);
```

`Custom Iterator`를 만들어서 사용할 경우, `Custom Iterator`에 영향을 받아 반복문의 순서가 변합니다.
인자 값을 수정할 경우, `fail-fast` 방식으로 `ConcurrentModificationException`이 발생합니다.

``` java
Consumer<String> removeElement = s -> {
    System.out.println(s + " " + list.size());
    if (s != null && s.equals("A")) {
        list.remove("D");
    }
};
```

``` java
list.stream().forEach(removeElement);
```

``` java
A 4
Exception in thread "main" java.util.ConcurrentModificationException
	at java.util.ArrayList.forEach(ArrayList.java:1252)
	at ReverseList.main(ReverseList.java:1)
```

위와 같이 수정 즉시, 바로 `ConcurrentModificationException`을 던집니다.

<br>

### Collections.stream.foreach()문 (java8 부터 도입)

``` java
list.stream().forEach(consumer);
```

`Custom Iterator`를 만들어서 사용할 경우, `Custom Iterator`에 의한 영향을 받지 않아 순서 또한, 변하지 않습니다.
인자 값을 수정할 경우, 모든 반복문을 수행한 후에 `ConcurrentModificationException`이 발생합니다.

``` java
A 4
B 3
C 3
null 3
Exception in thread "main" java.util.ConcurrentModificationException
	at java.util.ArrayList$ArrayListSpliterator.forEachRemaining(ArrayList.java:1380)
	at java.util.stream.ReferencePipeline$Head.forEach(ReferencePipeline.java:580)
	at ReverseList.main(ReverseList.java:1)
```
<br>

## 결론

단순한 반복문을 구현하는데에 있어 가장 속도가 빠른 순서는 `기존 For 문`-`향상된 For문` - `함수형 프로그래밍 방식의 For문`입니다.

`parallelStream()을 이용한 For문`이 `stream()을 이용한 For문`보다 빠를 수는 있으나, stream으로 변환하는 내부 라이브러리 비용이 있어 `기존 for문` 이나 `향상된 for문`보다 빠르지 않습니다.

`함수형 프로그래밍 패러다임을 적용한 For문`은 장점과 단점이 있으므로 상황에 맞게 적절히 사용하는 것이 중요합니다.

<br>

### 참고 사이트

* [https://springframework.guru/java-8-foreach/](https://springframework.guru/java-8-foreach/)
* [https://www.baeldung.com/java-collection-stream-foreach](https://www.baeldung.com/java-collection-stream-foreach)
* [https://stackoverflow.com/questions/16635398/java-8-iterable-foreach-vs-foreach-loop/16635489](https://stackoverflow.com/questions/16635398/java-8-iterable-foreach-vs-foreach-loop/16635489)
* [https://stackoverflow.com/questions/33582050/what-is-a-difference-between-traditional-loop-and-for-each-loop](https://stackoverflow.com/questions/33582050/what-is-a-difference-between-traditional-loop-and-for-each-loop)
* [https://stackoverflow.com/questions/11555418/why-is-the-enhanced-for-loop-more-efficient-than-the-normal-for-loop](https://stackoverflow.com/questions/11555418/why-is-the-enhanced-for-loop-more-efficient-than-the-normal-for-loop)
* [https://stackoverflow.com/questions/20375176/should-i-always-use-a-parallel-stream-when-possible](https://stackoverflow.com/questions/20375176/should-i-always-use-a-parallel-stream-when-possible)
* [https://homoefficio.github.io/2016/06/26/for-loop-%EB%A5%BC-Stream-forEach-%EB%A1%9C-%EB%B0%94%EA%BE%B8%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%ED%95%A0-3%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0/](https://homoefficio.github.io/2016/06/26/for-loop-%EB%A5%BC-Stream-forEach-%EB%A1%9C-%EB%B0%94%EA%BE%B8%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%ED%95%A0-3%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0/)
* [https://blog.jooq.org/2015/12/08/3-reasons-why-you-shouldnt-replace-your-for-loops-by-stream-foreach/](https://blog.jooq.org/2015/12/08/3-reasons-why-you-shouldnt-replace-your-for-loops-by-stream-foreach/)
* [https://www.4te.co.kr/876](https://www.4te.co.kr/876)

