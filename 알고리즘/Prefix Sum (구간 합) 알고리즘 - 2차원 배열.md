# Prefix Sum (구간 합) 알고리즘 - 2차원 배열

2차원 배열에서의 구간 합이란, 2차원 배열 array가 존재할 때, index (x1, y1) 부터 (x2, y2) 까지의 직사각형 범위의 구간 합을 의미합니다.

<br>

### 일반적인 방법으로 구간 합 계산

array 배열의 크기를 N이라고 가정합니다.

빈복문으로 (x1, y1) 부터 (x2, y2) 까지의 array 합을 sum이라는 변수에 누적해서 계산해주면, 시간복잡도는 O(N^2) 입니다.

<br>

중요한 것은 동일한 배열에 대해서 M개의 구간에 대해서 구간 합을 구할 때의 시간복잡도입니다.

이 때는 시간복잡도가 O(N^2 * M) 이 됩니다.

<br>

### Prefix Sum 으로 구간 합 계산

아이디어는 다음과 같습니다.

array 배열의 index 값의 시작은 1로 설정합니다.

이 때, 구간 합 배열은 다음을 의미합니다.

prefixSum\[a][b]: array\[1][1] 부터 array\[a][b] 까지의 합

<br>

위와 같이 구간 합 배열을 생성해놓으면, (x1, y1) ~ (x2, y2) 까지의 array 합을 구할 때, 드는 시간복잡도는 O(1) 이 됩니다. 또한, 동일한 배열에 대해서 M개의 구간에 대해서 구간 합을 구할 때의 시간복잡도는 O(M) 이 됩니다.

방법은 다음과 같습니다.

#### array[index] 값

| index | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0     | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| 1     | 0    | 6    | 5    | 4    | 3    | 2    | 1    |
| 2     | 0    | 5    | 4    | 3    | 2    | 1    | 0    |
| 3     | 0    | 4    | 3    | 2    | 1    | 0    | 0    |

<br>

#### prefixSum[index] 값

| index | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
| ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0     | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
| 1     | 0    | 6    | 11   | 15   | 18   | 20   | 21   |
| 2     | 0    | 11   | 20   | 27   | 32   | 35   | 36   |
| 3     | 0    | 15   | 27   | 32   | 35   | 36   | 36   |

<br>

구간 합 (array\[x1][y1] ~ array\[x2][y2] 까지의 합) = (prefixSum\[x2][y2] - prefixSum\[x1-1][y2] - prefixSum\[x2][y1 - 1] + prefixSum\[x1-1][y1 - 1]) 이 됩니다.

이를 이용하여 Prefix Sum을 이용하는 대표 문제인 [백준 11660번](https://www.acmicpc.net/problem/11660)을 해결하겠습니다.

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

        StringTokenizer tokenizer = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(tokenizer.nextToken());
        int M = Integer.parseInt(tokenizer.nextToken());

        int[][] array = new int[N + 1][N + 1];
        for (int i = 1; i <= N; i++) {
            tokenizer = new StringTokenizer(br.readLine());
            for (int j = 1; j <= N; j++) {
                array[i][j] = Integer.parseInt(tokenizer.nextToken());
            }
        }

        int[][] prefixSum = new int[N + 1][N + 1];
        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                prefixSum[i][j] = prefixSum[i - 1][j] + prefixSum[i][j - 1] - prefixSum[i - 1][j - 1] + array[i][j];
            }
        }

        for (int i = 0; i < M; i++) {
            tokenizer = new StringTokenizer(br.readLine());
            int x1 = Integer.parseInt(tokenizer.nextToken());
            int y1 = Integer.parseInt(tokenizer.nextToken());

            int x2 = Integer.parseInt(tokenizer.nextToken());
            int y2 = Integer.parseInt(tokenizer.nextToken());

            int answer = prefixSum[x2][y2] - prefixSum[x1 - 1][y2] - prefixSum[x2][y1 - 1] + prefixSum[x1 - 1][y1 - 1];
            bw.write(answer + "\n");
        }
        bw.flush();
    }
}
```

