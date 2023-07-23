# Meta Hacker Cup 2022 Qualification Round C1,C2 문제

### 문제 이해

각각의 codeword(부호어)가 서로의 접두사이면 안됩니다. 

그래야 encoding된 암호문이 decoding 될 때, 2가지 이상의 원문으로 해석되지 않기 때문입니다.

<br>

### 문제 풀이 방법

1. 만들 수 있는 모든 codeword(부호어)를 생성해봅니다.

   - codeword 최대 길이는 10입니다.

   - `.` 또는 `-` 문자를 이용하여 10자리의 codeword 를 만들면 최대 1024개의 문자열을 만들 수 있습니다.

2. 예비 codeword가 주어진 C1 문자와 서로 접두사 관계에 있지 않으면, 유일한 codeword 이므로 출력합니다.

   - C1 문자의 길이는 최대 100입니다.
   - 우리가 만든 codeword 의 길이는 최대 10입니다.

3. N개의 codeword를 출력하면 프로그램을 종료합니다.

<br>

### 시간복잡도 분석

- 테스트케이스 T: 95 ~ 100
- 예비 codeword 목록 생성 시, 시간복잡도: 2^10 = 1024
- 예비 codeword 1개와 C1이 서로 접두사 관계에 있는지 확인하는 시간복잡도: 예비 codeword의 길이 10과 C1 문자열의 길이 100 중 더 긴 100이 됩니다.
- 결론: O(100 * 1024 * 100) = O(1024만)

<br>

### 구현 코드 - java

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class C1 {
    public static void main(String[] args) throws IOException {
//        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedReader br = new BufferedReader(new FileReader("C:\\Github\\Algorithm-Contest\\test_data\\hackercup2022\\qualification\\C1\\second_meaning_input.txt"));
//        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedWriter bw = new BufferedWriter(new FileWriter("C:\\Github\\Algorithm-Contest\\test_data\\hackercup2022\\qualification\\C1\\second_meaning_output.txt"));

        int T = Integer.parseInt(br.readLine());
        for (int i = 0; i < T; i++) {
            int N = Integer.parseInt(br.readLine());
            String C1 = br.readLine();

            List<String> potentialCodewordList = new ArrayList<>();
            makePotentialCodewordList(new StringBuilder(), potentialCodewordList, 10);

            bw.write("Case #" + (i + 1) + ":\n");
            int countOfCodeword = 1;
            for (int j = 0; j < potentialCodewordList.size() && countOfCodeword < N; j++) {
                String potenatialCodeword = potentialCodewordList.get(j);
                if (!potenatialCodeword.startsWith(C1) && !C1.startsWith(potenatialCodeword)) {
                    bw.write(potenatialCodeword + "\n");
                    countOfCodeword++;
                }
            }
        }
        bw.flush();
    }

    private static void makePotentialCodewordList(StringBuilder codewordBuilder, List<String> codewordList, int lengthOfCodeword) {
        if (codewordBuilder.length() >= lengthOfCodeword) {
            codewordList.add(codewordBuilder.toString());
            return;
        }

        codewordBuilder.append(".");
        makePotentialCodewordList(codewordBuilder, codewordList, lengthOfCodeword);
        codewordBuilder.deleteCharAt(codewordBuilder.length() - 1);

        codewordBuilder.append("-");
        makePotentialCodewordList(codewordBuilder, codewordList, lengthOfCodeword);
        codewordBuilder.deleteCharAt(codewordBuilder.length() - 1);
    }
}
```

