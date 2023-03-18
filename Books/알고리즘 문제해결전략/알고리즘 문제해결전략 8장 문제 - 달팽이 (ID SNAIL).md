# 알고리즘 문제해결전략 8장 문제 - 달팽이 (ID SNAIL)

### 문제링크: https://www.algospot.com/judge/problem/read/SNAIL

<br>

### 문제해석

1. 길이가 n미터인 우물의 맨바닥에 달팽이가 있습니다.

2. 달팽이의 움직임은 날씨에 따라 다릅니다.

   - 비가 오는 날에는 하루에 2미터씩 기어올라갈 수 있습니다.

   - 맑은 날에는 하루에 1미터씩 기어올라갈 수 있습니다.

3. m일 동안 모든 날짜에 비가 올 확률이 정확히 75%일 전망입니다.

4. m일 안에 달팽이가 우물 맨 위까지 올라갈 확률을 구하는 문제입니다.

<br>

### 문제 풀이 - 동적계획법 (Top-Down 방식)

1. 달팽이가 우물 맨 위까지 올라갈 확률 구하는 동적계획법 함수를 다음과 같이 설계합니다.
1. double getPossibilityOfReachTarget(int day, int height, int endDay, int targetHeight, double\[][] possibility)
   - day: 달팽이가 움직이기 시작한지 며칠이 지났는지를 나타내는 변수입니다.
   - height: 달팽이가 위치한 현재 높이를 의미합니다.
   - endDay: 달팽이가 움직이는 일수의 최댓값을 나타내는 변수입니다. (이 문제에서는 m 값으로 고정됩니다.)
   - targetHeight: 달팽이가 맨 위까지 올라가야하는 높이를 의미합니다. (이 문제에서는 n 값으로 고정됩니다.)
   - possibility\[dat][height]: 달팽이가 움직인지 day일이 지났을 때, 달팽이의 현재 위치가 height미터에 위치한다면, endDay일에 targetHeight까지 도달할 확률을 저장하는 메모이제이션 변수입니다.
   - possibility\[dat][height]은 day일에 비가 올 때의 확률과 맑은 날일 때의 확률로 나누고 이를 합산하여 계산합니다.
     - 0.75(= 비가 오는 날의 확률) X (day + 1 일, height + 2미터일 때, endDay일 내에 targetHeight미터까지 도달할 확률)
     - 0.25(= 맑은 날의 확률) X (day + 1 일, height + 1미터일 때, endDay일 내에 targetHeight미터까지 도달할 확률)
     - 위 2개의 사건은 독립적인 사건이며, 동시에 일어나는 사건이 아니므로 각각의 확률을 더해주면 됩니다.
   - 첫 번째 기저조건은 day가 endDay일을 지났을 때, endDay일 내에 targetHeight까지 도달하지 못한 것이므로 0을 반환합니다.
   - 두 번째 기저조건은 day가 endDay일 이내면서, height가 targetHeight보다 크거나 같을 때, endDay일 내에 targetHeight까지 도달한 것이므로 1을 반환합니다.

<br>

### 소스코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int i= 0; i < C; i++) {
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            int n = Integer.parseInt(tokenizer.nextToken());
            int m = Integer.parseInt(tokenizer.nextToken());

            double[][] possibility = new double[m + 1][n];
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k < n; k++) {
                    possibility[j][k] = -1;
                }
            }

            double answer = getPossibilityOfReachTarget(0, 0, m, n, possibility);
            bw.write(answer + "\n");
        }
        bw.flush();
    }

    private static double getPossibilityOfReachTarget(int day, int height, int endDay, int targetHeight, double[][] possibility) {
        if (day > endDay) {
            return 0;
        }

        if (height >= targetHeight) {
            return 1;
        }

        if (possibility[day][height] > -0.5) {
            return possibility[day][height];
        }

        double possibilityOfRainyDay = 0.75 * getPossibilityOfReachTarget(day + 1, height + 2, endDay, targetHeight, possibility);
        double possibilityOfSunnyDay = 0.25 * getPossibilityOfReachTarget(day + 1, height + 1, endDay, targetHeight, possibility);

        return possibility[day][height] = possibilityOfRainyDay + possibilityOfSunnyDay;
    }
}
```
