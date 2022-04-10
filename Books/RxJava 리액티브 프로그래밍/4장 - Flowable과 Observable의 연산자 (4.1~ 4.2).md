# 4장 - Flowable과 Observable의 연산자

## 4.1 Flowable/Observable을 생성하는 연산자

* just
인자를 순서대로 통지하는 Flowable을 생성 (최대 10개까지 가능)

``` java
Flowable<String> flowable = Flowable.just("A", "B", "C");
```

* fromArray
인자를 순서대로 통지하는 Flowable을 생성 (just와 다르게 쉼표로 무한하게 인자 개수 제한 없음)

``` java
Flowable<String> flowable = Flowable.fromArray("A", "B", "C");
```

* fromIterable
인자로 Iterable인터페이스를 구현한 객체를 받아서, 순서대로 통지하는 Flowable을 생성

``` java
Flowable<String> flowable = Flowable.fromIterable("A", "B", "C");
```

* fromCallable
인자로 받은 `Callable을 실행하여 반환하는 데이터`를 통지하는 Flowable을 생성
인자로 함수형 인터페이스를 구현한 객체를 받을 수 있으며, 반환 값이 하나이기 때문에 람다식을 받을 수 있습니다.

``` java
Flowable<String> flowable = Flowable.fromCallable(() -> System.currentTimeMillis());
```

* range(int start, int count)
`start부터 값이 1씩 증가하는 데이터를 count개` 통지하는 Flowable을 생성

``` java
Flowable<String> flowable = Flowable.range(10, 3);
// 10, 11, 12 를 통지합니다.
```

* rangeLong(long start, long count)
`start부터 값이 1씩 증가하는 데이터를 count개` 통지하는 Flowable을 생성
range와 기능은 같으나, 인자의 자료형이 long입니다.

``` java
Flowable<String> flowable = Flowable.rangeLong(10, 3);
// 10, 11, 12 를 통지합니다.
```

* interval
`지정한 시간 간격마다 0부터 1씩 증가하는 데이터`를 통지하는 Flowable을 생성
동일한 이름을 가지면서, 인자만 다른 메소드(메소드 오버로딩)는 4가지가 있습니다.
예시로는, interval(long initialDelay, long time, TimeUnit unit); 을 사용했습니다.

``` java
Flowable<String> flowable = Flowable.interval(2000L, 1000L, TimeUnit.MILLISECONDS);
// 2초 뒤부터 통지를 시작하며, 1초마다 0부터 1씩 증가한 데이터를 통지합니다.
// 2초: 0
// 3초: 1
// 4초: 2
```

* timer
`지정한 시간 이후에 0만 출력하고 종료`하는 Flowable을 생성
동일한 이름을 가지면서, 인자만 다른 메소드(메소드 오버로딩)는 2가지가 있습니다.
예시로는, timer(long time, TimeUnit unit); 을 사용했습니다.

``` java
Flowable<String> flowable = Flowable.timer(2000L, TimeUnit.MILLISECONDS);
// 2초 후에 0을 통지하고 종료합니다.
// 2초: 0
```

* defer
`Callable이 생성하는 Flowable에서 생성한 데이터`를 순서대로 통지하는 Flowable을 생성

``` java
Flowable<LocalTime> flowable = Flowable.defer(() -> Flowable.just(LocalTime.now()));

// 구독시점마다 다른 시간 값이 출력됩니다.
```

* empty
빈 Flowable을 생성하는 연산자

``` java
Flowable<String> flowable = Flowable.empty();
// 구독하자마자 onComplete을 통지합니다.
```

* error
에러 객체를 인자로 받아서 에러를 통지하는 Flowable을 생성하는 연산자

``` java
Flowable<String> flowable = Flowable.error(new Exception("예외 발생"));
// 구독하자마자 onError를 통지합니다.
```

* never
아무 것도 통지하지 않는 Flowable을 생성하는 연산자

``` java
Flowable<String> flowable = Flowable.empty();
// 어떤 것도 통지하지 않습니다. 완료도 통지하지 않습니다.
```

<br>

## 4.2 통지 데이터를 변환하는 연산자

* map
인자로 받은 Function 인터페이스를 이용하여 `통지하는 데이터를 변환`해주는 연산자

``` java
Flowable<String> flowable = Flowable.just("A", "B", "C")
    								.map(data -> data.toLowerCase());
// a, b, c를 통지
```

* flatMap
`map`과 기능은 비슷하나, map이 1개의 데이터만 변환하는 것과는 달리, flatMap 연산자는 0 \~ 여러개의 데이터로 변환할 수 있습니다.
이 때, 변환인자로 사용되는 Function 인터페이스는 Flowable/Observable을 반환하기 때문에 `Flowable.empty()` 함수를 사용하여 어떤 데이터도 반환하지 않을 수 있으며,
`Flowable.just(1, 2)` 를 사용하여 여러 데이터로도 반환할 수 있습니다.

``` java
Flowable<String> flowable = Flowable.just("A", "B", "C", "")
    								.flatMap(data -> {
                                        if(data.equals("")) {
                                            return Flowable.empty();
                                        } else {
                                            return Flowable.just(data.toLowerCase());
                                        }
                                    });
// 빈 문자열 "" 이 들어오면 변환하지 않습니다.
// 빈 문자열이 아닐 경우, 소문자로 변환
```