# LCS(Longest Common Subsequence) - 최장공통부분수열

### 정의

LCS란, 두 수열이 주어졌을 때, `두 수열 모두의 부분수열이 되는 수열 중 가장 긴 수열`을 말합니다.

<br>

### 예시

`ACAYKP` 와 `CAPCAK` 의 LCS는 `ACAK` 입니다.

<br>

### 동적계획법을 이용한 LCS 길이 구하기

DP\[i][j] : `문자열A의 1 ~ i번까지의 문자열` 과 `문자열B의 1 ~ j번까지의 문자열` 사이의 LCS 의 길이

이 표를 직접 채워넣으면서, 규칙성을 찾아보았습니다.

| 비교문자(인덱스) | A(1) | C(2) | A(3) | Y(4) | K(5) | P(6) |
| ---------------- | ---- | ---- | ---- | ---- | ---- | ---- |
| C(1)             | 0    | 1    | 1    | 1    | 1    | 1    |
| A(2)             | 1    | 1    | 2    | 2    | 2    | 2    |
| P(3)             | 1    | 1    | 2    | 2    | 2    | 3    |
| C(4)             | 1    | 2    | 2    | 2    | 2    | 3    |
| A(5)             | 1    | 2    | 3    | 3    | 3    | 3    |
| K(6)             | 1    | 2    | 3    | 3    | 4    | 4    |

<br>

1. `문자열A의 i번째 문자`와 `문자열B의 j번째 문자`가 같을 때

   - DP\[i][j] = DP\[i - 1][j - 1] + 1

   - (`문자열A의 1 ~ i번까지의 문자열` 과 `문자열B의 1 ~ j번까지의 문자열` 사이의 LCS 길이)

     = (현재 비교문자 이전 문자열까지의 LCS 길이) + 1

2. `문자열A의 i번째 문자`와 `문자열B의 j번째 문자`가 다를 때

   - DP\[i][j] = Math.max(DP\[i - 1][j], DP\[i][j - 1])

   - (`문자열A의 1 ~ i번까지의 문자열` 과 `문자열B의 1 ~ j번까지의 문자열` 사이의 LCS 길이)

     = (`문자열A의 1 ~ i - 1번까지의 문자열` 과 `문자열B의 1 ~ j번까지의 문자열` 사이의 LCS 길이) 와 (`문자열A의 1 ~ i번까지의 문자열` 과 `문자열B의 1 ~ j - 1번까지의 문자열` 사이의 LCS 길이) 중 더 큰 값

<br>

### 구현 소스코드

위의 조건을 소스로 구현했습니다.

[백준 9251번](https://www.acmicpc.net/problem/9251) 문제의 조건에 맞춰 구현하였습니다.

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String s1 = br.readLine();
        String s2 = br.readLine();

        int[][] LCS = new int[s1.length() + 1][s2.length() + 1];
        for (int i = 1; i <= s1.length(); i++) {
            for (int j = 1; j <= s2.length(); j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    LCS[i][j] = LCS[i - 1][j - 1] + 1;
                } else {
                    LCS[i][j] = Math.max(LCS[i - 1][j], LCS[i][j - 1]);
                }
            }
        }
        bw.append(LCS[s1.length()][s2.length()] + "");
        bw.flush();
    }
}
```

