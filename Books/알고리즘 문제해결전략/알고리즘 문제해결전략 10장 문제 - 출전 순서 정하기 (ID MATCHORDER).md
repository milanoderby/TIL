# 알고리즘 문제해결전략 10장 문제 - 출전 순서 정하기 (ID MATCHORDER)

### 문제링크: https://www.algospot.com/judge/problem/read/MATCHORDER

<br>

### 문제해석

1. 러시아팀과 한국팀의  경기가 있습니다.

1. 각각 N명의 선수들이 출전합니다.

1. 1:1 승부에서는 항상 레이팅이 높은 선수가 승리합니다. 레이팅이 동일하다면, 한국팀의 선수가 이깁니다.

1. 러시아팀 선수들의 출전순서를 알고 있을 때, 한국팀이 얻을 수 있는 최대 승수를 구하는 문제입니다.


<br>

### 문제 풀이 - 탐욕법

1. 러시아 선수 R이 있을 때, 이길 수 있는 2명의 한국선수를 K1, K2라고 합시다.
1. 이 때, 러시아 선수 R1을 상대하기 위해 출전하는 선수가 K1, K2 둘 중 하나라면, 한국팀이 이기는 결과에는 변화가 없습니다.
1.  그렇다면, 탐욕적으로 K1과 K2 중에 더 레이팅이 낮은 선수가 R을 상대하는 것이 한국팀의 승수를 쌓는데에 유리하다는 것 또한 사실입니다.
1. 예를 들어, 러시아선수 2명의 레이팅이 R1, R2 이고, 한국선수 2명의 레이팅이 K1, K2 이고, 각 레이팅의 대소관계는 아래와 같다고 합시다.
   - R1 < K1 < R2 < K2
   - 이 때 R1 : K2 / R2 : K1 으로 대진을 짜게 되면, R2가 K1보다 높으므로 한국팀은 1승 밖에 할 수 없습니다.
   - 하지만, R1 : K1 / R2 : K2 와 같이 K1과 K2의 출전순서를 바꾼다면 탐욕적으로 바꾼다면, 한국팀이 2승을 할 수 있게 됩니다.
   - 위와 같이 탐욕적인 방법으로 한국팀의 최대 승수를 구할 수 있습니다.

<br>

### 소스코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int i= 0; i < C; i++) {
            int N = Integer.parseInt(br.readLine());
            int[] russiaTeam = new int[N];
            int[] koreaTeam = new int[N];

            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                russiaTeam[j] = Integer.parseInt(tokenizer.nextToken());
            }

            tokenizer = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                koreaTeam[j] = Integer.parseInt(tokenizer.nextToken());
            }

            Arrays.sort(russiaTeam);
            Arrays.sort(koreaTeam);

            int indexOfRussia = 0;
            int indexOfKorea = 0;
            int answer = 0;
            while (indexOfRussia < N && indexOfKorea < N) {
                if (russiaTeam[indexOfRussia] <= koreaTeam[indexOfKorea]) {
                    indexOfRussia++;
                    indexOfKorea++;
                    answer++;
                } else {
                    indexOfKorea++;
                }
            }
            bw.write(answer + "\n");
        }
        bw.flush();
    }
}
```
