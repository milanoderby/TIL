# 알고리즘 문제해결전략 8장 문제 - 최대 증가 부분 수열 (ID LIS)

### 문제링크: https://www.algospot.com/judge/problem/read/LIS

<br>

### 문제해석

1. 정수 수열이 주어집니다.
1. 주어진 정수 수열에서 몇 개의 주어진 숫자를 삭제하고, 남은 숫자의 순서는 유지한 수열을 부분수열이라고 합니다.
1. 순증가하는 수열을 부분수열을 증가 부분 수열이라고 합니다.
1. 가장 길이가 긴 증가 부분 수열의 길이를 구하는 문제입니다.

<br>

### 문제 풀이 - 동적계획법 (Top-Down 방식)

1. 가장 길이가 긴 증가 부분 수열의 길이를 구하는 동적계획법 함수를 다음과 같이 설계합니다.
1. int getLengthOfLIS(int index, int[] numbers, int[] dp)
   - index 부터 시작하는 증가 부분 수열의 길이 중 가장 긴 것의 길이를 반환하는 함수입니다.
   - numbers: 주어진 정수 수열입니다.
   - dp: 중복된 계산을 막기 위해 이미 계산된 값을 저장하는 메모이제이션 배열입니다.
   - 현재 numbers[index]부터 시작하는 증가 부분 수열 중 가장 긴 것을 구하기 위해 아래와 같이 구현합니다.
   - 현재 index보다 뒤에 있는 숫자들을 순회합니다.
     - numbers[nextIndex]가 numbers[index] 보다 큰 숫자라면, getLengthOfLIS(nextIndex, numbers, dp) 의 길이를 구합니다.
     - numbers[nextIndex]로 시작하는 증가 부분 수열 중 가장 길이가 긴 것을 계산합니다.
     - 위에서 구한 증가 부분 수열의 길이에 현재 numbers[index]라는 숫자도 증가 부분 수열로 추가된 것이므로 길이를 1 더해줍니다.
1. 최종적으로는 모든 index를 순회하며, getLengthOfLIS(index, numbers, dp) 중에 가장 길이가 긴 것을 답으로 산출합니다.

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
            int N = Integer.parseInt(br.readLine());
            int[] numbers = new int[N];
            int[] dp = new int[N];
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                numbers[j] = Integer.parseInt(tokenizer.nextToken());
                dp[j] = -1;
            }

            int answer = -1;
            for (int j = 0; j < N; j++) {
                answer = Math.max(answer, getLengthOfLIS(j, numbers, dp));
            }
            bw.write(answer + "\n");
        }
        bw.flush();
    }

    // index 부터 시작하는 증가 부분 수열의 길이 중 가장 긴 것
    private static int getLengthOfLIS(int index, int[] numbers, int[] dp) {
        if (dp[index] != -1) {
            return dp[index];
        }

        int lengthOfLIS = 0;
        for (int nextIndex = index + 1; nextIndex < numbers.length; nextIndex++) {
            if (numbers[index] < numbers[nextIndex]) {
                lengthOfLIS = Math.max(lengthOfLIS, getLengthOfLIS(nextIndex, numbers, dp));
            }
        }

        return dp[index] = lengthOfLIS + 1;
    }
}
```
