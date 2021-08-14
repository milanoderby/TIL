# Java MultiValueMap

개발~~(이라고 쓰고, 보통은 PS)~~ 을 하다보면, 하나의 Key에 여러개의 Value를 저장하는 MultiValueMap이 필요할 때가 있습니다.

Java에서는 다양한 자료구조를 `java.util` 패키지를 통해 지원하지만, 안타깝게도, MultiValueMap은 지원하지 않습니다. 그래서, MultiValueMap을 구현하는 여러가지 방법을 찾아보았습니다.

<br>

### 1. Spring Framework 에서 지원하는 MultiValueMap 사용

공식문서: https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/MultiValueMap.html

<br>

### 2. Apache Commons 에서 지원하는 MultiValuedMap 사용

공식문서: https://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/MultiValuedMap.html

참고자료: https://www.baeldung.com/apache-commons-multi-valued-map

<br>

### 3. Java로 직접 구현

사실, 위의 2가지 방식 모두 내부적으로는 Java Collections로 Map<K, List\<V>> 형태로 구현한 것입니다.

 그렇다면, 왜 java.util 패키지에 구현해놓지 않았을까? 라는 의문이 들었는데, 고민해본 결과 다음과 같은 결론을 내렸습니다. 일반적으로, MultiValueMap을 구현했을 때, Map<K, List\<V>> 의 형태로 구현할 수도 있지만, 때에 따라서, List\<V> 형태로 Value들을 저장하는 것보다 다른 Collection으로 저장하는 것이 나은 상황이 있을 수도 있다는 생각이 들었습니다. 

 예를 들어, MultiValueMap에서 같은 Key를 가지는 Value들이 정렬되어 저장되고, 정렬된 채로 꺼내지기를 바란다면, 어떨까요? Value를 List에 저장할 경우, 정렬된 순서로 Value를 빼내고 싶을 때마다 Sort를 수행해야합니다. 이 과정은 매우 비효율적입니다.

 대신에 Value를 PriorityQueue 에 저장할 경우, 꺼낼 때마다 명시한 정렬순서대로 꺼내질 것입니다. 즉, 이런 점에 있어서 MultiValueMap에서 MultiValue를 저장할 Collection을 선택할 자유를 개발자에게 부여하기 위해 Java에서는 미리 MultiValueMap을 구현하지 않았다는게 맞을 것 같습니다.

<br>

### MultiValueMap 활용한 소스코드

다음은 [프로그래머스 베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579) 문제를 풀 때, MultiValueMap 을 사용하여 해결한 방법입니다.

사실, 이 문제는 이런 방식으로 해결하지 않아도 되지만(더 효율적인 방법이 있습니다.), MultiValueMap을 사용할 수 있는 코드라 생각하여 예시자료로 남깁니다.

```java
import java.util.*;
import java.util.stream.Collectors;
class Solution {
    public int[] solution(String[] genres, int[] plays) {
        List<Integer> answerList = new ArrayList<>();
        Map<String, Genre> genrePlayCountMap = new HashMap<>();
        Map<String, PriorityQueue<Song>> genreSongListMap = new HashMap<>();

        for (int i = 0; i < genres.length; i++) {
            Genre genre = genrePlayCountMap.getOrDefault(genres[i], new Genre(genres[i], 0));
            genre.countOfPlay += plays[i];
            genrePlayCountMap.put(genres[i], genre);

            PriorityQueue<Song> songList = genreSongListMap.getOrDefault(genres[i], new PriorityQueue<>((s1, s2) -> {
                if (s1.countOfPlay == s2.countOfPlay) {
                    return s1.id - s2.id;
                }
                return s2.countOfPlay - s1.countOfPlay;
            }));
            songList.add(new Song(i, plays[i]));
            genreSongListMap.put(genres[i], songList);
        }

        List<Genre> sortedGenreList = genrePlayCountMap.values().stream().sorted(Comparator.comparing(Genre::getCountOfPlay).reversed()).collect(Collectors.toList());

        for (Genre genre : sortedGenreList) {
            PriorityQueue<Song> songPriorityQueue = genreSongListMap.get(genre.name);

            int countOfSong = 0;
            while (!songPriorityQueue.isEmpty() && countOfSong < 2) {
                countOfSong++;
                Song bestSong = songPriorityQueue.poll();
                answerList.add(bestSong.id);
            }
        }

        return answerList.stream().mapToInt(Integer::intValue).toArray();
    }

    private static class Song {
        private int id;
        private int countOfPlay;

        public Song(int id, int countOfPlay) {
            this.id = id;
            this.countOfPlay = countOfPlay;
        }
    }

    private static class Genre {
        private String name;
        private long countOfPlay;

        public Genre(String name, long countOfPlay) {
            this.name = name;
            this.countOfPlay = countOfPlay;
        }

        public long getCountOfPlay() {
            return countOfPlay;
        }
    }
}
```

<br>

### 참고자료

https://stackoverflow.com/questions/30704568/multivaluemap-in-java

https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/MultiValueMap.html

https://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/MultiValuedMap.html

https://www.baeldung.com/apache-commons-multi-valued-map