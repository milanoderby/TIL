# LeetCode Contest 330 - 2551번 Put Marbles in Bags

### 문제

https://leetcode.com/contest/weekly-contest-330/problems/put-marbles-in-bags/

<br>

### 문제풀이1. DP 로 접근

시간복잡도를 계산하면, 상당히 오래 걸리는 방식이므로 당연히 안됩니다.

```
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) throws IOException, RuntimeException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        bw.write(putMarbles(new int[]{1,3}, 2) + "\n");
        bw.flush();
    }

    private static long putMarbles(int[] weights, int k) {
        long[][] maxScoreDp = new long[weights.length][k];
        long[][] minScoreDp = new long[weights.length][k];
        for (int i = 0; i < weights.length; i++) {
            for (int j = 0; j < k; j++) {
                maxScoreDp[i][j] = -1;
                minScoreDp[i][j] = -1;
            }
        }

        long maxScore = getMaxScore(0, weights, 0, k, maxScoreDp);
        long minScore = getMinScore(0, weights, 0, k, minScoreDp);

        return maxScore - minScore;
    }

    private static long getMaxScore(int startIndex, int[] weights, int countOfSelected, int k, long[][] maxScoreDp) {
        // 가방의 개수가 하나 남아 모두 담아야될 때의 비용
        if (countOfSelected == k - 1) {
            int last = weights.length - 1;

            if (startIndex <= last) {
                return (long) (weights[startIndex]) + weights[last];
            }
            return Integer.MIN_VALUE;
        }

        // 가방의 개수가 너무 많이 남은 상태
        // 한개의 가방에 최소 하나의 구슬이 있어야 합니다.
        if (startIndex >= weights.length) {
            return Integer.MIN_VALUE;
        }

        if (maxScoreDp[startIndex][countOfSelected] != -1) {
            return maxScoreDp[startIndex][countOfSelected];
        }

        long maxScore = 0;
        for (int nextIndex = startIndex; nextIndex < weights.length; nextIndex++) {
            maxScore = Math.max(maxScore, getMaxScore(nextIndex + 1, weights, countOfSelected + 1, k, maxScoreDp) + weights[startIndex] + weights[nextIndex]);
        }

        return maxScoreDp[startIndex][countOfSelected] = maxScore;
    }

    private static long getMinScore(int startIndex, int[] weights, int countOfSelected, int k, long[][] minScoreDp) {
        // 가방의 개수가 하나 남아 모두 담아야될 때의 비용
        if (countOfSelected == k - 1) {
            int last = weights.length - 1;

            if (startIndex <= last) {
                return (long) (weights[startIndex]) + weights[last];
            }
            return Integer.MAX_VALUE;
        }

        // 가방의 개수가 너무 많이 남은 상태
        // 한개의 가방에 최소 하나의 구슬이 있어야 합니다.
        if (startIndex >= weights.length) {
            return Integer.MAX_VALUE;
        }

        if (minScoreDp[startIndex][countOfSelected] != -1) {
            return minScoreDp[startIndex][countOfSelected];
        }

        long minScore = Integer.MAX_VALUE;
        for (int nextIndex = startIndex; nextIndex < weights.length; nextIndex++) {
            minScore = Math.min(minScore, getMinScore(nextIndex + 1, weights, countOfSelected + 1, k, minScoreDp) + weights[startIndex] + weights[nextIndex]);
        }

        return minScoreDp[startIndex][countOfSelected] = minScore;
    }
}
```

<br>

### 문제풀이2. 탐욕법으로 접근

k개의 구간으로 나눈다는 것은 결국 (k-1) 개의 경계를 가진다는 것과 동일합니다.

이러한 방식으로 접근하면, (k-1) 개의 경계를 정하고, 해당 경계의 값들을 모두 더했을 때, 최댓값과 최솟값을 구하면 됩니다.

<br>

```
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) throws IOException, RuntimeException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        bw.write(putMarbles(new int[]{1,3,5,1}, 2) + "\n");
        bw.flush();
    }

    private static long putMarbles(int[] weights, int k) {
        List<Long> boundaryValueList = new ArrayList<>();
        for (int i = 0; i < weights.length - 1; i++) {
            long boundaryValue = weights[i] + weights[i + 1];
            boundaryValueList.add(boundaryValue);
        }

        long maxScore = weights[0] + weights[weights.length - 1];
        Collections.sort(boundaryValueList, Comparator.reverseOrder());
        for (int i = 0; i < k - 1; i++) {
            maxScore += boundaryValueList.get(i);
        }

        long minScore = weights[0] + weights[weights.length - 1];
        Collections.sort(boundaryValueList);
        for (int i = 0; i < k - 1; i++) {
            minScore += boundaryValueList.get(i);
        }

        return maxScore - minScore;
    }
}
```

