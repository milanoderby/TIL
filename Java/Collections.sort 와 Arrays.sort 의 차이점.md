# Collections.sort 와 Arrays.sort 의 차이점



## Arrays.sort()

Dual-Pivot Quicksort를 사용합니다.

이는 Stable Sort가 아닙니다.

즉, 같은 값일 경우, 정렬 전/후 순서가 바뀔 수도 있습니다.



## Collections.sort()

mergesort를 사용합니다.

이는 Stable Sort 입니다.



## 참고

- https://stackoverflow.com/questions/32334319/why-does-collections-sort-use-mergesort-but-arrays-sort-does-not