# 알고리즘 문제해결전략 8장 문제 - 타일링 방법의 수 세기 (ID TILING2)

### 문제링크: https://www.algospot.com/judge/problem/read/TILING2

<br>

### 문제해석

1. 2xn 크기의 사각형이 주어집니다.
1. 주어진 사각형을 2x1 크기의 사각형으로 빈틈없이 채우는 경우의 수를 구하는 문제입니다.

<br>

### 문제 풀이 - 동적계획법 (Top-Down 방식)

1. 2x1 크기의 사각형으로 빈틈없이 채우는 경우의 수를 구하는 동적계획법 함수를 다음과 같이 설계합니다.
1. int getCountOfPossibleTiling(int horizontalLength, int[] dp)
   - horizontalLength: 주어지는 사각형의 가로 길이를 말합니다.
   - dp: 중복된 계산을 막기 위해 이미 계산된 값을 저장하는 메모이제이션 배열입니다.
   - 2 x horizontalLength 크기의 사각형을 2 x 1 크기의 사각형으로 채울 때, 다음과 같이 생각합니다.
     - 먼저, 2 x 1 크기의 사각형을 세로로 놓고 시작합니다. 이 때, 남은 칸인 horizontalLength - 1 칸을 채우는 경우의 수를 계산합니다.
     - 두 번째로, 2 x 1 크기의 사각형을 가로로 놓고 시작합니다. 이 때, 남은 칸인 horizontalLength - 2 칸을 채우는 경우의 수를 계산합니다.
     - 첫 사각형을 어떻게 놓느냐에 따라 2가지의 서로 다른 사건이 발생합니다.
     - 이 때, 2가지의 사건은 각각 동시에 발생하는 사건이 아니므로 별개의 사건으로 볼 수 있습니다.
     - 따라서, 2가지 사건의 경우의 수를 더하면 현재 사건의 경우의 수를 계산할 수 있습니다.
     - getCountOfPossibleTiling(horizontalLength, dp) = getCountOfPossibleTiling(horizontalLength - 1, dp) + getCountOfPossibleTiling(horizontalLength - 2, dp)
1. 위의 수식에서 모듈라 연산을 계속 취해주는 방식을 이용하면 답을 구할 수 있습니다.

<br>

### 소스코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    private static int DIVISOR = 1_000_000_007;
    private static int MAX_LENGTH = 100;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int[] dp = new int[MAX_LENGTH + 1];
        for (int index = 1; index <= MAX_LENGTH; index++) {
            dp[index] = -1;
        }
        dp[1] = 1;
        dp[2] = 2;

        int C = Integer.parseInt(br.readLine());
        for (int i = 0; i < C; i++) {
            int horizontalLength = Integer.parseInt(br.readLine());
            int answer = getCountOfPossibleTiling(horizontalLength, dp);
            bw.write(answer + "\n");
        }
        bw.flush();
    }

    private static int getCountOfPossibleTiling(int horizontalLength, int[] dp) {
        if (dp[horizontalLength] != -1) {
            return dp[horizontalLength];
        }

        int countOfPossibleTiling = (getCountOfPossibleTiling(horizontalLength - 1, dp) + getCountOfPossibleTiling(horizontalLength - 2, dp)) % DIVISOR;
        return dp[horizontalLength] = countOfPossibleTiling;
    }
}
```
