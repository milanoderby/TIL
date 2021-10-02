# Floyd's tortoise and hare (플로이드의 사이클 탐지 알고리즘)

### 동작원리

리스트에서 순회속도가 다른 2개의 포인터를 이용합니다.

이 때, 순회속도가 빠른 포인터를 토끼, 순회속도가 느린 포인터를 거북이에 빗대어 토끼와 거북이 알고리즘이라고 표현한 것입니다.

일반적으로, Linked List 에서 Cycle을 탐지하기 위한 알고리즘입니다.

Linked List에서 Cycle이 없다면, 순회속도가 빠른 포인터는 null에 먼저 도달하여 순회속도가 느린 포인터와 만날 일이 없습니다.

반면, Linked List에 Cycle이 존재한다면, 순회속도가 빠른 포인터는 순회속도가 느린 포인터와 어느 지점에서 만나게 되어있습니다.

이를 통해 Cycle의 존재여부를 탐지할 수 있습니다. 추가적으로 Cycle의 시작지점 역시 파악할 수 있습니다.

<br>

### Cycle의 시작지점 찾기

이 때, 순회속도가 빠른 포인터와 느린 포인터가 만나는 지점(교차점)을 이용해 Cycle이 시작되는 지점을 알아낼 수 있습니다. 다음은, Cycle 시작지점을 찾는 원리를 설명합니다.

![pic](./images/floyd-cycle-detection-1)

예를 들어, 2번 지점에서 토끼와 거북이가 동일한 지점에서 동시에 출발하며, 속도는 토끼가 거북이의 2배라고 하겠습니다.

위의 그림에서 나타낸 것처럼 변수를 정의하겠습니다.

**출발지점부터 Cycle 시작지점까지의 거리**를 **F** 라고 정의합니다.

**Cycle시작지점부터 토끼와 거북이가 처음 만나는 교차점까지의 거리**를 **a** 라고 정의합니다.

**Cycle의 길이**를 **C** 라고 정의합니다.

토끼와 거북이는 각각 Cycle을 N바퀴와 M바퀴를 돌고나서, 교차점에 만난다고 가정하면, 다음의 공식이 세워집니다.
$$
F + N \times C + a = 2\times(F + M \times C + a) \\
(N - 2 \times M) \times C = F + a \\
여기서, \ N - 2 \times M은 \ 자연수이므로 \ 자연수 \ K로 \ 표현할 \ 수 \ 있습니다.\\
F + a = K \times C \\
F = K \times C - a
$$
여기서, 위치적으로 생각할 때, Cycle위의 교차점인 **K * C - a** 는 K의 값에 상관 없이 항상 같은 지점인 **C - a**를 가리키게 됩니다. 즉, Cycle이 형성되는 상황에서 **F = C - a** 라는 식이 성립하게 됩니다.

토끼와 거북이가 만난 **교차점부터 Cycle 시작지점까지의 거리**는 **C - a** 이며, 이 거리는 **출발지점부터 Cycle 시작지점까지의 거리**인 **F**와 같습니다. 

![pic](./images/floyd-cycle-detection-2)

결론적으로, 토끼와 거북이가 만날 때, 거북이만 다시 출발지점으로 이동시키고, 토끼와 거북이가 같은 속도로 이동했을 때, 다시 만나는 지점이 Cycle의 시작지점입니다.

<br>

### 문제

https://leetcode.com/problems/find-the-duplicate-number/

위는 Cycle의 시작지점을 찾기를 활용한 문제입니다.

배열의 숫자는 N + 1개이며, 배열요소의 값은 1 ~ N 범위의 속합니다.

또한, 하나의 숫자만 중복된다는 조건이 있습니다.

이 문제는 하나의 중복되는 숫자를 찾는 문제입니다.

<br>

특정 요소만 중복이 된다는 것은 다음을 의미합니다.

배열의 요소 값을 다음 요소의 index로 사용하면서, 다음 요소를 찾을 때, 특정 요소는 2번 이상 가리키게 됩니다. 그리고, 이 요소가 Cycle형성의 시작지점이 됩니다.

<br>

### 소스코드

위 문제풀이에 대한 소스코드입니다.

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int tortoise = nums[0];
        int hare = nums[0];

        do {
            tortoise = nums[tortoise];
            hare = nums[hare];
            hare = nums[hare];
        } while (tortoise != hare);

        tortoise = nums[0];
        while (tortoise != hare) {
            tortoise = nums[tortoise];
            hare = nums[hare];
        }

        return hare;
    }
}
```

<br>

### 참고자료

https://leetcode.com/problems/find-the-duplicate-number/solution/

