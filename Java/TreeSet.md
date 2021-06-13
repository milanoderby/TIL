# Java TreeSet

java 1.2부터 지원된 Collections Framework의 멤버 클래스이다.

이진검색트리(Binary Search Tree) 자료구조의 형태로 데이터를 저장한다.

TreeSet의 구현은 `레드-블랙 트리`로 되어있다.



## 시간복잡도

| 조회     | 추가     | 삭제     |
| -------- | -------- | -------- |
| O(log N) | O(log N) | O(log N) |



## 메소드

| 종류 | 메소드명                          | 설명                                                         |
| ---- | --------------------------------- | ------------------------------------------------------------ |
| 추가 | add(E e)                          | Element를 추가한다.                                          |
|      | addAll(Collection<? extends E> c) | Collection의 모든 Element들을 추가한다.                      |
| 조회 | first()                           | 가장 첫번째 Element를 반환한다.                              |
|      | last()                            | 가장 마지막 Element를 반환한다.                              |
|      | floor(E e)                        | Element보다 작거나 같은 값 중 가장 큰 값를 반환한다.<br />없다면, null을 반환한다. |
|      | lower(E e)                        | Element보다 작은 값 중 가장 큰 값을 반환한다.<br />없다면, null을 반환한다. |
|      | ceiling(E e)                      | Element보다 크거나 같은 값 중 가장 작은 값을 반환한다.<br />없다면, null을 반환한다. |
|      | higher(E e)                       | Element보다 큰 값 중 가장 작은 값을 반환한다.<br />없다면, null을 반환한다. |
|      | contains(Object o)                | Element가 Set에 포함하고 있으면 true, 그 외는 false를 반환한다. |
|      | isEmpty()                         | Element를 가지고 있지 않다면, true를 그 외는 false를 반환한다. |
|      | size()                            | Element의 개수를 반환한다.                                   |
| 제거 | pollFirst()                       | 가장 첫번째 Element를 반환하고, 제거한다.                    |
|      | pollLast()                        | 가장 마지막 Element를 반환하고, 제거한다.                    |
|      | remove(Object o)                  | 인자로 주어진 Element를 제거한다.                            |
|      | clear()                           | 모드 Element를 제거한다.                                     |



## 주의사항

treeset 에서 탐색 시, 값은 반환하나, 위치 값(index)은 반환하지 않는다.

그렇기 때문에 위치 값을 찾아야하는 이진탐색은 불가하다

이진탐색을 직접 구현하거나,

배열을 정렬 후, TreeMap<Integer, Integer> 을 통해 TreeMap<값, 인덱스> 형태로 저장을 하여 탐색을 하도록 한다.
