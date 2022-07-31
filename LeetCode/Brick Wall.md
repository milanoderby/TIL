# Brick Wall

### 개념

직사각형 벽이 있습니다.

직사각형 벽은 n 개의 행을 가지며, 각 행을 이루는 벽돌들의 높이는 1로 동일하지만, 각 벽돌의 너비는 다릅니다.

이 때, 벽을 세로 방향으로 자른다고 가정하면, 각 벽돌 중 잘려지는 벽돌이 있을 수도 있고, 반대로 벽돌 사이의 틈으로 지나가서 잘리지 않는 벽돌이 있을 수도 있습니다.

그렇다면, 벽돌을 세로 방향으로 잘랐을 때, 벽돌의 너비 중 어느 부분을 잘라야 가장 최소로 벽돌을 자를 수 있을지, 그 때 최소로 잘리는 벽돌의 개수는 몇 개인지 구하는 문제입니다.

<br>

### 문제풀이

1. 예제 1번의 그림을 보면, 벽돌을 세로로 잘랐을 때, 잘리는 부분과 잘리지 않는 부분을 알 수 있습니다.
   - 예제의 입력 값은 아래와 같습니다.
   - `wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]`
   - 그림은 아래와 같습니다.
   - ![img](./images/brick-wall-1)
   - 이 때, 벽돌을 각 행 단위로 보면, 벽돌의 너비를 순차적으로 더한 값에서는 세로로 잘라도 잘리지 않는 다는 것을 알 수 있습니다.
   - 예를 들어, 제일 위층인 `[1,2,2,1]` 을 순차적으로 더한 값인 `[1, 3, 5, 6]` 에서는 벽돌을 수직으로 잘라도 잘리지 않습니다.
   - 우리가 구해야하는 상황은 최소로 벽돌이 잘릴 때의 잘리는 벽돌의 갯수이므로 이를 반대로 말하면, (벽돌 층의 개수) - (최대로 벽돌이 잘리지 않을 때의 잘리는 갯수) 를 구하면 됩니다.
   - 다만, 벽돌의 시작 값과 가장 끝 값은 제외하고 계산해야됩니다.

<br>

### 구현

```java
class Solution {
    public int leastBricks(List<List<Integer>> wall) {
        int widthOfWall = 0;
        Map<Integer, Integer> countOfPassableBrickMap = new HashMap<>();
        for (List<Integer> brick : wall) {
            int indexOfBrick = 0;
            for (Integer widthOfBrick : brick) {
                indexOfBrick += widthOfBrick;
                countOfPassableBrickMap.compute(indexOfBrick,
                    (key, previousCount) -> previousCount == null ? 1 : previousCount + 1);
            }
            widthOfWall = indexOfBrick;
        }

        int maximumCountOfPassableBrick = 0;
        for (Integer indexOfBrick : countOfPassableBrickMap.keySet()) {
            if (indexOfBrick == 0 || indexOfBrick == widthOfWall) {
                continue;
            }
            maximumCountOfPassableBrick = Math.max(maximumCountOfPassableBrick,
                countOfPassableBrickMap.get(indexOfBrick));
        }
        return wall.size() - maximumCountOfPassableBrick;
    }
}
```

