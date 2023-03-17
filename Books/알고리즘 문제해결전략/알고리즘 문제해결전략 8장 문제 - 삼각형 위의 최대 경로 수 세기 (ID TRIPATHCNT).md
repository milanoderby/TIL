# 알고리즘 문제해결전략 8장 문제 - 삼각형 위의 최대 경로 수 세기 (ID TRIPATHCNT)

### 문제링크: https://www.algospot.com/judge/problem/read/TRIPATHCNT

<br>

### 문제해석

1. 삼각형 모양으로 배치된 자연수들이 있습니다.
1. 맨 위의 숫자에서 시작해, 한 번에 한 줄씩 아래로 내려가 맨 아래 줄로 내려가는 경로를 만들려고 합니다. 
1. 아래 줄로 내려갈 때, 바로 아래 숫자 또는 오른쪽 아래 숫자로 내려갈 수 있습니다.
1. 이 때, 각 경로에 포함된 숫자의 합이 최대가 되는 경로의 갯수를 구하는 문제입니다.

<br>

### 문제 풀이 - 동적계획법 (Top-Down 방식)

1. 각 경로에 포함된 숫자의 합 중 최댓값을 구하는 동적계획법 함수를 다음과 같이 설계합니다.
1. int getMaxSumOfPath(int row, int col, int\[][] triangle, int\[][] dp)
   - 현재 좌표 (row, col)에서 시작하여 아래로 내려가는 경로의 합 중 최댓값을 구하는 함수입니다.
   - dp\[row][col]: 현재 좌표 (row, col)에서 시작해 맨 아랫줄에 도달할 때까지 구한 경로의 합을 저장하는 메모이제이션 변수입니다.
   - 다음 좌표는 바로 아래 또는 오른쪽 아래 좌표입니다.
   - dp\[row][col]은 현재 위치의 숫자(= triangle\[row][col])와 다음 좌표에서 시작하며 아래로 내려가는 경로의 합 2가지 경우(= max(getMaxSumOfPath(row + 1, col, ...), getMaxSumOfPath(row + 1, col + 1, ...))) 중 더 큰 값을 선택하여 더해주면 됩니다.
   - 기저조건은 row가 삼각형의 크기보다 클 때이며, 이 때는 0을 반환합니다. (경로의 합을 더해줘야하는데 이 때, 실제 경로의 합 계산에 영향이 없는 0을 더해줍니다.)
1. int getCountOfMaxSumPath(int row, int col, int\[][] triangle, int\[][] dp, int\[][] count)
   - 현재 좌표 (row, col)에서 시작하여 아래로 내려가는 경로의 합이 최대가 되는 경로의 개수를 구하는 함수입니다.
   - count\[row][col]: 현재 좌표 (row, col)에서 시작하여 아래로 내려가는 경로의 합이 최대가 되는 경로의 개수를 저장하는 메모이제이션 변수입니다.
   - 다음 좌표는 바로 아래 또는 오른쪽 아래 좌표입니다.
   - count\[row][col]은 현재 위치의 숫자(= triangle\[row][col])와 다음 좌표에서 시작하며 아래로 내려가는 경로의 합 2가지 경우 중 경로의 합이 최대가 되는 경로가 있다면, 해당 좌표에서 시작하는 경로의 합이 최대가 되는 경로의 개수(= getCountOfMaxSumPath(row + 1, 다음 좌표의 col 값, ...))를 더해줍니다.
   - 기저조건은 row가 삼각형의 크기보다 클 때이며, 이 때는 1을 반환합니다. (경로의 개수 하나가 늘은 것이므로)

<br>

### 소스코드

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int i = 0; i < C; i++) {
            int sizeOfTriangle = Integer.parseInt(br.readLine());
            int[][] triangle = new int[sizeOfTriangle][sizeOfTriangle];
            int[][] dp = new int[sizeOfTriangle][sizeOfTriangle];
            int[][] count = new int[sizeOfTriangle][sizeOfTriangle];

            for (int row = 0; row < sizeOfTriangle; row++) {
                StringTokenizer tokenizer = new StringTokenizer(br.readLine());
                for (int col = 0; col <= row; col++) {
                    triangle[row][col] = Integer.parseInt(tokenizer.nextToken());
                    dp[row][col] = -1;
                    count[row][col] = -1;
                }
            }

            getMaxSumOfPath(0, 0, triangle, dp);
            int answer = getCountOfMaxSumPath(0, 0, triangle, dp, count);
            bw.write(answer + "\n");
        }
        bw.flush();
    }

    private static int getMaxSumOfPath(int row, int col, int[][] triangle, int[][] dp) {
        if (row >= triangle.length) {
            return 0;
        }

        if (dp[row][col] != -1) {
            return dp[row][col];
        }

        int sumOfDownPath = getMaxSumOfPath(row + 1, col, triangle, dp);
        int sumOfDownRightPath = getMaxSumOfPath(row + 1, col + 1, triangle, dp);

        return dp[row][col] = triangle[row][col] + Math.max(sumOfDownPath, sumOfDownRightPath);
    }

    private static int getCountOfMaxSumPath(int row, int col, int[][] triangle, int[][] dp, int[][] count) {
        if (row == triangle.length - 1) {
            return 1;
        }

        if (count[row][col] != -1) {
            return count[row][col];
        }

        int countOfMaxSumPath = 0;
        if (getMaxSumOfPath(row, col, triangle, dp) == triangle[row][col] + getMaxSumOfPath(row + 1, col, triangle, dp)) {
            countOfMaxSumPath += getCountOfMaxSumPath(row + 1, col, triangle, dp, count);
        }

        if (getMaxSumOfPath(row, col, triangle, dp) == triangle[row][col] + getMaxSumOfPath(row + 1, col + 1, triangle, dp)) {
            countOfMaxSumPath += getCountOfMaxSumPath(row + 1, col + 1, triangle, dp, count);
        }

        return count[row][col] = countOfMaxSumPath;
    }
}
```
