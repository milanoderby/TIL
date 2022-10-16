# Suffix Array (접미사 배열) 알고리즘

### 정의

문자열 S가 주어질 때, S의 접미사들은 아래와 같이 정의합니다.

index는 앞으로도 zero-based index를 사용하겠습니다.

S[i]: 문자열 S의 i번째 문자부터 시작하는 접미사 (여기서, 0 <= i < 문자열 S의 길이)

문자열 S = banana로 가정할 때, S의 i번째 문자로 시작하는 접미사 Suffix는 아래와 같습니다.

| i    | Suffix[i] (S의 i번째 문자로 시작하는 접미사) |
| ---- | -------------------------------------------- |
| 0    | banana                                       |
| 1    | anana                                        |
| 2    | nana                                         |
| 3    | ana                                          |
| 4    | na                                           |
| 5    | a                                            |

<br>

이 때, **접미사 배열의 정의는 접미사의 목록을 사전순으로 나열한 배열**을 의미합니다.

접미사 배열을 나타내는 SuffixArray[j]의 정의는 아래와 같습니다.

| j    | SuffixArray[j] |
| ---- | -------------- |
| 0    | a              |
| 1    | ana            |
| 2    | anana          |
| 3    | banana         |
| 4    | na             |
| 5    | nana           |

하지만, 위와 같이 **SuffixArray 배열에 실제 문자열로 저장하는 방식은 메모리를 과다하게 사용합니다!**

그래서, 위와 같이 저장하는 대신, SuffixArray[j] 에는 접미사 Suffix의 시작 index 값을 이용하여 저장합니다.

<br>

| j    | SuffixArray[j] |
| ---- | -------------- |
| 0    | 5              |
| 1    | 3              |
| 2    | 1              |
| 3    | 0              |
| 4    | 4              |
| 5    | 2              |

<br>

### 접미사 배열을 구하는 방법 - 1. Naive 한 방법

#### 구현방법

1. 모든 접미사들을 List 자료구조에 저장합니다.
2. List 자료구조에 저장한 접미사들을 문자열 순서로 정렬합니다.
3. 문자열 순서로 정렬한 접미사 List를 SuffixArray 배열로 변환합니다.

<br>

#### 시간복잡도 분석

1. 문자열 S의 길이를 N 이라할 때, O(N) 입니다.
2. 1. List 원소의 수가 N이라 할 때, List 자료구조를 정렬할 때 원소끼리 비교하는 횟수는 O(N * log N) 입니다.
   2. 한쌍의 문자열 중 어떤 문자열이 사전순으로 앞선지 비교할 때 소모되는 시간복잡도는 O(N) 입니다.
   3. 결과적으로 문자열을 사전순으로 비교할 때 소모되는 시간복잡도는 O(N^2 * log N) 입니다.
3. List를 Array로 옮겨담으면 되므로 시간복잡도는 O(N) 입니다.

1번 + 2번 + 3번의 시간복잡도를 합쳐서 계산하면, **이 방법의 전체 시간복잡도는 O(N^2 * log N)** 입니다.

<br>

#### 구현코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String S = br.readLine();
        StringBuilder sb = new StringBuilder(S);
        List<String> suffixList = new ArrayList<>();

        for (int i = 0; i < S.length(); i++) {
            suffixList.add(sb.toString());
            sb.deleteCharAt(0);
        }

        Collections.sort(suffixList);
        for (String s : suffixList) {
            bw.write(s + "\n");
        }

        int[] suffixArray = new int[S.length()];
        for (int i = 0; i < suffixList.size(); i++) {
            suffixArray[i] = S.length() - suffixList.get(i).length();
        }

        for (int j = 0; j < suffixArray.length; j++) {
            bw.write(j + " " + suffixArray[j] + "\n");
        }

        bw.flush();
    }
}
// 입력값
banana
// 출력값
a
ana
anana
banana
na
nana
0 5
1 3
2 1
3 0
4 4
5 2
```

<br>

### 접미사 배열을 구하는 방법 - 2. Manber-Myers 알고리즘

#### 구현방법

이 방법은 **접미사라는 특성을 이용**하여 비교적 적은 메모리와 시간을 사용하여 접미사 배열을 구하는 방법입니다.

1. 문자열 S의 접미사 목록들을 첫번째 문자열을 기준으로 정렬을 수행합니다.

   여기서, 접미사 목록들의 첫 번째 문자열은 결국 문자열 S[i] (여기서 0 <= i < 문자열S의 길이) 라는 점을 이용하면 쉽게 정렬을 할 수 있습니다. 

   1. Suffix[i] 의 첫번째 문자(**문자열 S[i] 기준과 동일**)까지를 기준으로 정렬

      | i      | Suffix[i]  |
      | ------ | ---------- |
      | 0      | **b**anana |
      | 1      | **a**nana  |
      | 2      | **n**ana   |
      | 3      | **a**na    |
      | 4      | **n**a     |
      | 5      | **a**      |
      | 6 이후 | 빈 문자열  |

   2. SuffixArray 는 사전순으로 정렬한 접미사 배열을 의미하며, 이 때, SuffixArray[j]에 저장된 값은 S에서 접미사의 시작 index를 의미합니다.

   3. SuffixArray에 저장된 정렬의 순서를 이용하여 rank배열에 저장합니다. 이 때, rank[j]는 다음을 의미합니다. 

      문자열 S의 j번째 문자로 시작하는  접미사의 정렬 순서 (숫자가 작을 수록 사전순으로 앞에 해당)

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] |
      | -------------------------------------------- | -------------- | -------------------- |
      | 0                                            | **a**nana, 1   | 0                    |
      | 1                                            | **a**na, 3     | 0                    |
      | 2                                            | **a**, 5       | 0                    |
      | 3                                            | **b**anana, 0  | 1                    |
      | 4                                            | **n**ana, 2    | 2                    |
      | 5                                            | **n**a, 4      | 2                    |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   |

   <br>

2. 이제 접미사 목록들의 두 번째 문자까지 비교하는 것으로 확장해보겠습니다.

   접미사 목록 중 2개의 문자열을 임의로 꺼내어서 두 번째 문자까지 비교한다고 가정해봅시다.

   <br>

   예를 들어, ana와 anana를 두번째 문자까지 사전순으로 정렬하기 위해 비교하겠습니다.

   이 때, 이미 우리는 첫 번째 문자까지 비교한 정보를 rank배열에 가지고 있습니다.

   ana는 SuffixArray[1] = 3에 해당하는 값입니다. rank[3] = 0입니다.

   anana는 SuffixArray[0] = 1에 해당하는 값입니다. rank[1] = 0입니다.

   첫번째 문자까지 rank 비교를 한 것으로는 순서를 정할 수 없으므로 두번째 문자까지 비교하겠습니다.

   <br>

   두 번째 문자끼리의 비교는 어떻게 할 수 있을까요? 바로 접미사의 특성을 이용합니다.

   ana의 두번째 문자는 na의 첫번째 문자입니다.

   anana의 두번째 문자는 nanan의 첫번째 문자입니다.

   즉, 위의 ana와 anana의 두번째 문자끼리의 비교연산은 이미 구했던 na의 첫번째 문자와 nana의 첫번째 문자를 비교하는 연산과 동일합니다.

   즉, rank[3 + 1] 과 rank[1 + 1]을 비교하면 됩니다.

   rank[4] = 2, rank[2] = 2 입니다.

   따라서. anana와 ana의 순서 비교는 동일한 순서이기 때문에 명확한 정렬순서가 정해지지 않습니다.

   <br>

   만약, a와 ana 문자열의 두번째 문자까지 비교하게 된다면, 어떨까요?

   이 때, 이미 우리는 첫 번째 문자까지 비교한 정보를 rank배열에 가지고 있습니다.

   a 는 SuffixArray[2] = 5에 해당하는 값입니다. rank[5] = 0입니다.

   ana는 SuffixArray[1] = 3에 해당하는 값입니다. rank[3] = 0입니다.

   첫번째 문자까지 rank 비교를 한 것으로는 순서를 정할 수 없으므로 두번째 문자까지 비교하겠습니다.

   <br>

   a 의 두번째 문자는 빈 문자열입니다.

   ana 의 두번째 문자는 na 의 첫번째 문자입니다.

   여기서, a는 두번째 문자열이 없으므로 직접 비교할 수 없습니다. 

   **하지만, 사전순으로 정렬을 하는 것이 목표이기 때문에 rank[5 + 1] = -1로 설정해둔 것입니다!**

   그래서 위에서 하던 방식 대로 동일한 비교연산을 취해줍니다.

   즉, rank[5 + 1] 과 rank[1 + 1]을 비교하면 됩니다.

   rank[6] = -1, rank[2] = 2 이기 때문에 rank 값이 더 작은 a가 ana 보다 더 앞선 순서로 정렬이 수행될 것 입니다.

   1. 두번째 문자까지 정렬 전의 비교과정을 살펴보겠습니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 1] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**nana, 1   | 0                    | 2                        |
      | 1                                            | **a**na, 3     | 0                    | 2                        |
      | 2                                            | **a**, 5       | 0                    | -1                       |
      | 3                                            | **b**anana, 0  | 1                    | 0                        |
      | 4                                            | **n**ana, 2    | 2                    | 0                        |
      | 5                                            | **n**a, 4      | 2                    | 0                        |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

   2. 위의 rank[suffixArray[j]]와 rank[suffixArray[j] + 1] 을 이용하여 SuffixArray를 정렬합니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j + 1]] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**, 5       | 0                    | -1                       |
      | 1                                            | **an**ana, 1   | 0                    | 2                        |
      | 2                                            | **an**a, 3     | 0                    | 2                        |
      | 3                                            | **ba**nana, 0  | 1                    | 0                        |
      | 4                                            | **na**na, 2    | 2                    | 0                        |
      | 5                                            | **na**, 4      | 2                    | 0                        |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

   3. 정렬이 완료된 SuffixArray를 바탕으로 rank 배열을 갱신합니다. (SuffixArray의 rank 갱신 시, 기존 rank배열을 사용하기 때문에 새로운 배열을 생성하여야 합니다.)

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j + 1]] | newRank[SuffixArray[j]] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ | ----------------------- |
      | 0                                            | **a**, 5       | 0                    | -1                       | 0                       |
      | 1                                            | **an**ana, 1   | 0                    | 2                        | 1                       |
      | 2                                            | **an**a, 3     | 0                    | 2                        | 1                       |
      | 3                                            | **ba**nana, 0  | 1                    | 0                        | 2                       |
      | 4                                            | **na**na, 2    | 2                    | 0                        | 3                       |
      | 5                                            | **na**, 4      | 2                    | 0                        | 3                       |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       | -1                      |

   4. 이어서 네번째 문자까지 정렬 전의 비교과정을 살펴보겠습니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 2] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**, 5       | 0                    | -1                       |
      | 1                                            | **anan**a, 1   | 1                    | 1                        |
      | 2                                            | **ana**, 3     | 1                    | 0                        |
      | 3                                            | **bana**na, 0  | 2                    | 3                        |
      | 4                                            | **nana**, 2    | 3                    | 3                        |
      | 5                                            | **na**, 4      | 3                    | -1                       |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

   5. 위의 rank[suffixArray[j]]와 rank[suffixArray[j] + 2] 을 이용하여 SuffixArray를 정렬합니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 2] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**, 5       | 0                    | -1                       |
      | 1                                            | **ana**, 3     | 1                    | 0                        |
      | 2                                            | **anan**a, 1   | 1                    | 1                        |
      | 3                                            | **bana**na, 0  | 2                    | 3                        |
      | 4                                            | **na**, 4      | 3                    | -1                       |
      | 5                                            | **nana**, 2    | 3                    | 3                        |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

   6. 정렬이 완료된 SuffixArray를 바탕으로 rank 배열을 갱신합니다. (SuffixArray의 rank 갱신 시, 기존 rank배열을 사용하기 때문에 새로운 배열을 생성하여야 합니다.)

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 2] | newRank[SuffixArray[j]] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ | ----------------------- |
      | 0                                            | **a**, 5       | 0                    | -1                       | 0                       |
      | 1                                            | **ana**, 3     | 1                    | 0                        | 1                       |
      | 2                                            | **anan**a, 1   | 1                    | 1                        | 2                       |
      | 3                                            | **bana**na, 0  | 2                    | 3                        | 3                       |
      | 4                                            | **na**, 4      | 3                    | -1                       | 4                       |
      | 5                                            | **nana**, 2    | 3                    | 3                        | 5                       |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       | -1                      |

   7. 이어서 8번째 문자까지 정렬 전의 비교과정을 살펴보겠습니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 4] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**, 5       | 0                    | -1                       |
      | 1                                            | **ana**, 3     | 1                    | -1                       |
      | 2                                            | **anana**, 1   | 2                    | 0                        |
      | 3                                            | **banana**, 0  | 3                    | 4                        |
      | 4                                            | **na**, 4      | 4                    | -1                       |
      | 5                                            | **nana**, 2    | 5                    | -1                       |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

   8. 위의 rank[suffixArray[j]]와 rank[suffixArray[j] + 4] 을 이용하여 SuffixArray를 정렬합니다.

      | j                                            | SuffixArray[j] | rank[SuffixArray[j]] | rank[SuffixArray[j] + 4] |
      | -------------------------------------------- | -------------- | -------------------- | ------------------------ |
      | 0                                            | **a**, 5       | 0                    | -1                       |
      | 1                                            | **ana**, 3     | 1                    | -1                       |
      | 2                                            | **anana**, 1   | 2                    | 0                        |
      | 3                                            | **banana**, 0  | 3                    | 4                        |
      | 4                                            | **na**, 4      | 4                    | -1                       |
      | 5                                            | **nana**, 2    | 5                    | -1                       |
      | SuffixArray[j]가 S의 길이보다 크거나 같을 때 | 없음           | -1                   | -1                       |

<br>

#### 시간복잡도 분석

1. 문자열 S의 길이를 N 이라할 때, 각각의 접미사 목록은 다음의 과정을 거치며, 접미사 배열을 생성합니다.

   1. 1번째 문자까지을 비교하여 SuffixArray 생성
   2. 2^1번째 문자열끼리 비교하여 SuffixArray 생성
   3. 2^k번째 문자열끼리 비교하여 SuffixArray 생성

   2^k 가 N의 길이보다 작을 때까지 수행하므로 O(log N) 번 SuffixArray를 생성

2. 하나의 SuffixArray를 생성 및 정렬하는데 걸리는 시간

   1. 한쌍의 문자열 정렬순서 비교 시, 걸리는 시간은 O(1) (rank 함수를 이용하므로)
   2. 하나의 SuffixArray를 정렬하는데 걸리는 시간은 O(N * log N) 입니다.

1번 + 2번의 시간복잡도를 합쳐서 계산하면, **이 방법의 전체 시간복잡도는 O(N * log^2 N)** 입니다.

<br>

#### 구현코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    private static int diff;
    private static int[] rank;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String S = br.readLine();

        List<Integer> suffixArray = new ArrayList<>();
        for (int i = 0; i < S.length(); i++) {
            suffixArray.add(i);
        }

        Collections.sort(suffixArray, Comparator.comparing(S::charAt, Character::compareTo));
        rank = new int[4 * S.length()];
        Arrays.fill(rank, -1);

        initRankArray(S, suffixArray, rank);

        Comparator<Integer> rankComparator = (idx1, idx2) -> {
            if (rank[idx1] != rank[idx2]) {
                return Integer.compare(rank[idx1], rank[idx2]);
            }
            return Integer.compare(rank[idx1 + diff], rank[idx2 + diff]);
        };

        for (int k = 1; k < S.length(); k *= 2) {
            diff = k;
            Collections.sort(suffixArray, rankComparator);
            rank = updateRankArray(suffixArray, rankComparator);
        }

        for (int i = 0; i < suffixArray.size(); i++) {
            bw.write(suffixArray.get(i) + " ");
        }
        bw.flush();
    }

    private static int[] updateRankArray(List<Integer> suffixArray, Comparator<Integer> rankComparator) {
        int[] newRank = new int[rank.length];
        Arrays.fill(newRank, -1);
        int count = 0;
        newRank[suffixArray.get(0)] = count;

        for (int currentIndex = 1; currentIndex < suffixArray.size(); currentIndex++) {
            int prevIndex = currentIndex - 1;
            if (rankComparator.compare(suffixArray.get(prevIndex), suffixArray.get(currentIndex)) != 0) {
                count++;
            }
            newRank[suffixArray.get(currentIndex)] = count;
        }
        return newRank;
    }

    private static void initRankArray(String S, List<Integer> suffixArray, int[] rank) {
        int firstIndex = suffixArray.get(0);
        int count = 0;
        rank[firstIndex] = count;

        for (int i = 1; i < suffixArray.size(); i++) {
            int prevIndex = suffixArray.get(i - 1);
            int currentIndex = suffixArray.get(i);
            if (S.charAt(prevIndex) != S.charAt(currentIndex)) {
                count++;
            }
            rank[currentIndex] = count;
        }
    }
}
```

<br>

### 참고자료

https://ko.wikipedia.org/wiki/%EC%A0%91%EB%AF%B8%EC%82%AC_%EB%B0%B0%EC%97%B4