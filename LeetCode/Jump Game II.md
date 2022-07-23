# Jump Game II

### 개념

여러 계단이 있고, 각 계단마다 최대로 점프할 수 있는 계단의 숫자가 적혀있습니다.

이 때, 최종 계단까지 도달할 수 있는 최소 점프 횟수를 구하는 문제입니다.

<br>

### 문제풀이

1. 각 계단을 도착할 수 있는 최소 점프 횟수를 기록하는 배열을 생성합니다.
2. 첫 번째 계단은 0번만에 도착할 수 있으므로 무조건 0입니다.
3. 그 외의 계단은 나올 수 있는 가장 큰 수로 설정합니다.
4. 첫 번째 계단에서부터 다음을 수행합니다.
   - 현재 계단에서 갈 수 있는 계단을 모두 순회합니다.
   - (다음 목적지 계단에 기록되어있던 최소 점프횟수) > (현재 계단까지의 최소점프횟수) + 1 일 경우, 
   - (다음 목적지 계단에 기록되어있던 최소 점프횟수) 를 (현재 계단까지의 최소점프횟수) + 1 로 갱신합니다.
5. 결론적으로 답은 (최종 계단에 기록되어있는 최소 점프횟수)가 될 것입니다.

<br>

### 구현

```java
class Solution {
    public int jump(int[] nums) {
        int[] countOfJump = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            countOfJump[i] = Integer.MAX_VALUE;
        }

        countOfJump[0] = 0;
        for (int src = 0; src < nums.length; src++) {
            for (int jump = 1; jump <= nums[src]; jump++) {
                int dest = src + jump;
                if (dest >= nums.length) {
                    continue;
                }
                
                countOfJump[dest] = Math.min(countOfJump[dest], countOfJump[src] + 1);
            }
        }
        
        return countOfJump[nums.length - 1];
    }
}
```

