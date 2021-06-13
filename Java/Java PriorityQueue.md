# Java PriorityQueue

이하, PQ로 부르겠습니다.



### 기본정보

java 1.5 버전부터 지원

상속: AbstractCollection\<E>, AbstractQueue\<E>, PriorityQueue\<E>

구현: Serializable, Iterable\<E>, Collection\<E>, Queue\<E>



### 특징

1. 우선순위 힙 기반 자료구조입니다.

2. 요소로 null 값을 가질 수는 없습니다.
3. 내부적으로 PQ의 크기를 관리하는 `capacity` 라는 변수가 있으며, 이는 PQ 크기만큼 커집니다. PQ에 요소가 추가될 때마다, `capacity`가 커집니다.
4. 멀티스레드 환경에서 동시성을 보장하지 않습니다. 멀티스레드 환경에서는 thread-safe 한 `PriorityBlockingQueue` 를 사용해야합니다.



### 우선순위 정렬순서

1. 기본적인 정렬순서`(natural ordering)` 는 `Primitive Type` 에서는 오름차순 정렬입니다.
2. 기본 생성자에 의해 생성된 PQ 요소들은 기본적인 정렬순서로(`natural ordering`) 정렬됩니다.
3. `Comparator`를 인자로 받는 생성자에 의해 생성된 PQ 요소들은 인자로 전달되는 `Comparator` 에 정의된 규칙(`compareTo 메소드`)에 따라 정렬됩니다.
4. 기본 정렬순서 (`natural ordering`)를 사용하는 PQ 에 `비교할 수 없는 객체`를 추가하는 것을 허용하지 않고, 예외를 발생시킵니다.



### 요소 추출/제거순서

`정렬된 순서에서 가장 작은 요소(첫 번째 값)` 를 추출/제거합니다.

이 순서에서 가장 첫 번째로 추출/제거될 값을 `head` 값이라고 지칭합니다.



### 메소드 별 시간복잡도

| 수행작업종류 | 메소드             | 메소드 설명                                                  | 시간복잡도 |
| ------------ | ------------------ | ------------------------------------------------------------ | ---------- |
| 조회         | peek()             | 요소를 추출하여 반환합니다.<br />PQ가 비어있으면, null을 반환합니다. | O(1)       |
|              | contains(Object o) | 인자로 전달된 요소를 포함하면, true를 반환합니다.            | O(N)       |
| 추가         | add(E e)           | 인자로 전달된 요소를 추가합니다.                             | O(log N)   |
|              | offer(E e)         | 인자로 전달된 요소를 추가합니다.                             | O(log N)   |
| 삭제         | poll()             | 요소를 추출하여 반환하고, PQ에서 제거합니다.<br />PQ가 비어있으면, null을 반환합니다. | O(log N)   |
|              | remove(Object o)   | 인자로 전달된 요소를 PQ에서 제거합니다.                      | O(N)       |

#### 주의사항

일반적으로 생각할 때, 전체 순회할 것으로 생각되는 `contains(Object o)`, `remove(Object o)` 메소드의 시간복잡도는 O(N * log N) 이여야 합니다. 

그러나, Oracle Docs 에서 O(N) 으로 나타낸 것 봐서는, 내부적으로 `LinkedList` 와 결합되어 구현됐다고 생각됩니다.



### 소스코드 예제

```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class Main{
    private static int V, E;
    public static void main(String[] args) {
        PriorityQueue<Person> minExpSalPQ = new PriorityQueue<>(new PathComparator());
        minExpSalPQ.add(new Person(3000, 1));
        minExpSalPQ.add(new Person(8000, 9));
        minExpSalPQ.add(new Person(4000, 1));
        minExpSalPQ.add(new Person(7000, 7));
        minExpSalPQ.add(new Person(6000, 3));
        minExpSalPQ.add(new Person(6000, 7));
        minExpSalPQ.add(new Person(5000, 4));

        while(!minExpSalPQ.isEmpty()) {
            Person person = minExpSalPQ.poll();
            System.out.println(person.experience + "년차, 연봉: " + person.salary + "만원");
        }
    }

    private static class Person {
        private int salary;
        private int experience;

        public Person(int salary, int experience) {
            this.salary = salary;
            this.experience = experience;
        }
    }

    private static class PathComparator implements Comparator<Person> {

        @Override
        public int compare(Person p1, Person p2) {
            if (p1.experience == p2.experience) {
                return p1.salary - p2.salary;
            }
            return p1.experience - p2.experience;
        }
    }
}
```

무작위로 연차와 연봉정보를 PQ에 추가한 후, 추출한 결과입니다.

PQ의 정렬순서는 연차 오름차순, 연봉 오름차순입니다.



#### 결과

```
1년차, 연봉: 3000만원
1년차, 연봉: 4000만원
3년차, 연봉: 6000만원
4년차, 연봉: 5000만원
7년차, 연봉: 6000만원
7년차, 연봉: 7000만원
9년차, 연봉: 8000만원
```



### 참고자료

https://docs.oracle.com/javase/7/docs/api/java/util/PriorityQueue.html