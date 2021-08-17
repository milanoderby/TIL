# Java Comparator 활용한 객체 정렬 - 클래스 구현

### Comparator Interface

Java 객체를 비교할 때, 비교 기준을 만들 때, 사용하는 인터페이스입니다.

이 인터페이스를 구현하려면, compare 메소드를 구현해야합니다.



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

