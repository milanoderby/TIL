# Java Comparator 활용한 객체 정렬 - 람다표현식

### Comparator Interface

Java 객체를 비교할 때, 비교 기준을 만들 때, 사용하는 인터페이스입니다.

이 인터페이스를 구현하려면, compare 메소드를 구현해야합니다.

이전에는 클래스를 직접 구현하고, 객체를 생성하는 방식을 사용했습니다.

이 문서에서는 Comparator 인터페이스가 **함수형 인터페이스**라는 사실을 이용하여 **람다표현식**으로 구현하겠습니다.

<br>

### 함수형 인터페이스





### Collections.sort(Collection<T> list, Comparator<? super T> c)

Java 에서 객체 Collection을 정렬할 때, 사용하는 메소드입니다.

정렬할 객체와 객체 정렬 기준(Comparator 인터페이스를 구현한 객체)를 인자로 받습니다.



### 사용 예시

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        List<Entry> entryList = new ArrayList<>();
        for (int index = 0; index < N; index++) {
            entryList.add(new Entry(index, Integer.parseInt(br.readLine())));
        }
        Collections.sort(entryList, new EntryComparator());
    }

    private static class Entry {
        int originIndex;
        int value;

        public Entry(int originIndex, int value) {
            this.originIndex = originIndex;
            this.value = value;
        }
    }

    private static class EntryComparator implements Comparator<Entry> {

        @Override
        public int compare(Entry o1, Entry o2) {
            return o1.value - o2.value;
        }
    }
}
```

<br>

### 참고자료

https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html#package.description

https://www.baeldung.com/java-8-sort-lambda

