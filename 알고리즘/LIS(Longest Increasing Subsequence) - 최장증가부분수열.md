# LIS(Longest Increasing Subsequence) - 최장증가부분수열

## 개념

### 정의

LIS란, 어떤 수열이 주어졌을 때, `부분수열이 되는 수열 중 오름차순으로 정렬된 가장 긴 수열`을 말합니다.

<br>

### 예시

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 4입니다.

<br>

## LIS 길이 구하기

### 1번 방법 - 동적계획법

#### 시간복잡도 O(N^2)

#### 개념 정의

문제를 풀기 위해 아래와 같이 정의하겠습니다.

A: 주어지는 수열, 숫자배열로 저장하겠습니다. 수열의 i번째 숫자는 A[i] 로 표현하겠습니다.

LIS[i] : A[i]를 마지막 원소로 가지는 수열 중 최장증가부분수열

lengthOfLIS[i] : A[i]를 마지막 원소로 가지는 수열 중 최장증가부분수열의 길이

<br>

#### 규칙성 찾기

A = {10, 20, 10, 30, 20, 50} 으로 주어졌을 때, LIS[i] 를 i = 1 일 때부터 채워나가겠습니다.

1. i = 1일 때

   | i              | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 0    | 10   | 20   | 10   | 30   | 20   | 50   |
   | LIS[i]         |      | 10   |      |      |      |      |      |
   | lengthOfLIS[i] |      | 1    |      |      |      |      |      |

2. i = 2일 때

   | i              | 0    | 1    | 2      | 3    | 4    | 5    | 6    |
   | -------------- | ---- | ---- | ------ | ---- | ---- | ---- | ---- |
   | A[i]           | 0    | 10   | 20     | 10   | 30   | 20   | 50   |
   | LIS[i]         |      | 10   | 10, 20 |      |      |      |      |
   | lengthOfLIS[i] |      | 1    | 2      |      |      |      |      |

3. i = 3일 때

   | i              | 0    | 1    | 2      | 3    | 4    | 5    | 6    |
   | -------------- | ---- | ---- | ------ | ---- | ---- | ---- | ---- |
   | A[i]           | 0    | 10   | 20     | 10   | 30   | 20   | 50   |
   | LIS[i]         |      | 10   | 10, 20 | 10   |      |      |      |
   | lengthOfLIS[i] |      | 1    | 2      | 1    |      |      |      |

4. i = 4일 때

   | i              | 0    | 1    | 2      | 3    | 4          | 5    | 6    |
   | -------------- | ---- | ---- | ------ | ---- | ---------- | ---- | ---- |
   | A[i]           | 0    | 10   | 20     | 10   | 30         | 20   | 50   |
   | LIS[i]         |      | 10   | 10, 20 | 10   | 10, 20, 30 |      |      |
   | lengthOfLIS[i] |      | 1    | 2      | 1    | 3          |      |      |

5. i = 5일 때

   | i              | 0    | 1    | 2      | 3    | 4          | 5      | 6    |
   | -------------- | ---- | ---- | ------ | ---- | ---------- | ------ | ---- |
   | A[i]           | 0    | 10   | 20     | 10   | 30         | 20     | 50   |
   | LIS[i]         |      | 10   | 10, 20 | 10   | 10, 20, 30 | 10, 20 |      |
   | lengthOfLIS[i] |      | 1    | 2      | 1    | 3          | 2      |      |

6. i = 6일 때

   | i              | 0    | 1    | 2      | 3    | 4          | 5      | 6              |
   | -------------- | ---- | ---- | ------ | ---- | ---------- | ------ | -------------- |
   | A[i]           | 0    | 10   | 20     | 10   | 30         | 20     | 50             |
   | LIS[i]         |      | 10   | 10, 20 | 10   | 10, 20, 30 | 10, 20 | 10, 20, 30, 50 |
   | lengthOfLIS[i] |      | 1    | 2      | 1    | 3          | 2      | 4              |

<br>

정의에 의해서 LIS[i] 는 A[i] 를 마지막원소로 가지는데, 이 때, LIS[i]는 오름차순 수열이기 때문에, A[i]는 해당 수열에서 가장 큰 값이 됩니다.

이를 역으로 생각하면, LIS[i] 를 구하는 방법은 A[i] 보다 작은 값으로 끝나는 LIS 중 가장 길이가 긴 수열에 A[i] 를 추가해주면 됩니다.

이를 이용하면, lengthOfLIS 배열을 1부터 N까지 채워나갈 수 있는데, 그 중 가장 큰 값이 최장증가부분수열의 길이가 됩니다.

<br>

#### 구현 소스코드

이를 소스코드로 구현하면 다음과 같습니다.

[백준 11053번](https://www.acmicpc.net/problem/11053) 문제의 조건을 이용하여 정답을 출력하는 소스코드입니다.

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());
        int[] A = new int[N + 1];
        StringTokenizer tokenizer = new StringTokenizer(br.readLine());
        for (int i = 1; i <= N; i++) {
            A[i] = Integer.parseInt(tokenizer.nextToken());
        }

        int[] lengthOfLIS = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            int maxLengthOfPrevLIS = 0;
            for (int j = 0; j <= i - 1; j++) {
                if (A[j] < A[i]) {
                    maxLengthOfPrevLIS = Math.max(maxLengthOfPrevLIS, lengthOfLIS[j]);
                }
            }
            lengthOfLIS[i] = maxLengthOfPrevLIS + 1;
        }

        int answer = 0;
        for (int i = 1; i <= N; i++) {
            answer = Math.max(answer, lengthOfLIS[i]);
        }
        bw.append(answer + "");
        bw.flush();
    }
}
```

<br>

### 2번 방법 - 이분탐색

#### 시간복잡도 O(N logN)

#### 개념 정의

A: 주어지는 수열, 숫자배열로 저장하겠습니다. 수열의 i번째 숫자는 A[i] 로 표현하겠습니다.

LIS[i] : A[i]를 마지막 원소로 가지는 수열 중 최장증가부분수열

lengthOfLIS[i] : A[i]를 마지막 원소로 가지는 수열 중 최장증가부분수열의 길이

<br>

#### 1번 방법에서 시간복잡도를 줄이는 방법

1번 방법에서는 `j < i 와 A[j] < A[i] 를 만족하는 j에 대해 LIS[j] 중 최장수열`의 뒤에 A[i]를 붙이는 것으로 LIS[i] 를 구했습니다.

즉, LIS[i]를 구하기 위해서 A[0] ~ A[i - 1] 값과 LIS[0] ~ LIS[i - 1] 값을 훑어보는 순차탐색(시간복잡도: O(N))을 이용하였습니다. 

<br>

이번에는, LIS[i] 를 구하기 위해서 LIS[j] = k 를 만들 수 있는 최소 A[j] 값의 뒤에 A[i] 의 값을 붙이는 방식으로 생각해보겠습니다. 이를 위해 `LIS[j]의 길이가 k를 만족하는 A[j] 중 최솟값` 을 k번째 인덱스에 A[j] 값을 저장하는 배열을 새로 정의하겠습니다. 

`minNumOfLISLength[k]` : `길이가 k인 LIS[j]를 만드는 A[j] 값들 중 최솟값`

<br>

그리고, 위에서 정의한 배열은 오름차순으로 정렬이 되는 배열입니다. 왜냐하면, LIS의 오름차순 특성을 따르기 때문입니다. 

예를 들면, A = {10, 5, 7} 인 경우

1. A[1] 번째 원소까지 생각했을 때, 길이 1의 LIS를 만드는 A[j]의 최솟값은 10입니다.
2. A[2] 번째 원소까지 생각했을 때, 길이 2의 LIS를 만드는 A[j] 는 없고, 길이 1의 LIS를 만드는 A[j]의 최솟값은 5로 갱신됩니다.
3. A[3] 번째 원소까지 생각했을 때, 길이 2의 LIS를 만드는 A[j] 는 7로 갱신되고, 길이 1의 LIS를 만드는 A[j]의 최솟값은 5입니다.

<br>

이를 이용하여, A[i] 로 만들 수 있는 LIS의 길이는 아래의 수식을 만족하는 k 를 찾으면 됩니다.

`minNumOfLISLength[k] < A[i] <= minNumOfLISLength[k + 1]`

길이 k의 LIS를 만드는 A[j] 값 보다 A[i] 값이 크므로 LIS[j] 수열에서 A[i] 라는 값을 추가해 (k+1) 의 길이를 가지는 LIS를 만들 수 있게됩니다.

<br>

이 때, 위의 조건을 만족하는 k 값을 찾을 때, minNumOfLISLength 배열은 오름차순 정렬이 되어있으므로 이분탐색을 이용할 수 있고, 이분탐색으로 인해 시간복잡도는 O(N) 에서 O(log N) 으로 줄어들게 됩니다.

<br>

#### 규칙성 찾기

A = {3, 5, 7, 9, 2, 1, 4, 8} 으로 주어졌을 때, LIS[i] 를 i = 1 일 때부터 채워나가겠습니다.

1. i = 1일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    |      |      |      |      |      |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 3    |      |      |      |      |      |      |      |

   길이 1의 LIS를 만드는 A[i]의 값 중 최솟값은 3 입니다.

2. i = 2일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    |      |      |      |      |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 3    | 5    |      |      |      |      |      |      |

   길이 2의 LIS를 만드는 A[i]의 값 중 최솟값은 5 입니다.

3. i = 3일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    |      |      |      |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 3    | 5    | 7    |      |      |      |      |      |

   길이 3의 LIS를 만드는 A[i]의 값 중 최솟값은 7 입니다.

4. i = 4일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    | 4    |      |      |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 3    | 5    | 7    | 9    |      |      |      |      |

   길이 4의 LIS를 만드는 A[i]의 값 중 최솟값은 9 입니다.

5. i = 5일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    | 4    | 1    |      |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | `2`  | 5    | 7    | 9    |      |      |      |      |

   길이 1의 LIS를 만드는 A[i]의 값 중 최솟값은 2로 갱신됩니다.

6. i = 6일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    | 4    | 1    | 1    |      |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | `1`  | 5    | 7    | 9    |      |      |      |      |

   길이 1의 LIS를 만드는 A[i]의 값 중 최솟값은 1로 갱신됩니다.

7. i = 7일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    | 4    | 1    | 1    | 2    |      |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 1    | `2`  | 7    | 9    |      |      |      |      |

   길이 2의 LIS를 만드는 A[i]의 값 중 최솟값은 2로 갱신됩니다.

8. i = 8일 때

   | i              | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | A[i]           | 3    | 5    | 7    | 9    | 2    | 1    | 4    | 8    |
   | lengthOfLIS[i] | 1    | 2    | 3    | 4    | 1    | 1    | 2    | 4    |

   | k                    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
   | -------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
   | minNumOfLISLength[k] | 1    | 2    | 7    | `8`  |      |      |      |      |

   길이 4의 LIS를 만드는 A[i]의 값 중 최솟값은 8로 갱신됩니다.

<br>

위의 규칙성을 통해 계속해서 `minNumOfLISLength` 배열을 갱신해나갈 수 있습니다.

1. `minNumOfLISLength[k] < A[i] <= minNumOfLISLength[k+1]` 을 만족하는 k를 찾습니다.

2. 위의 식을 만족하는 k 값을 찾고나면, `minNumOfLISLength[k + 1]`의 값을 `A[i]` 로 갱신해줍니다. 왜냐하면, 길이 k의 LIS 수열의 뒤에 A[i] 값을 붙여서, 길이 k+1의 LIS 수열을 만들 수 있기 때문에 minNumOfLISLength[k + 1]에 들어갈 값은 A[i] 가 되는 것입니다.

3. 이분탐색을 사용하는 경우, `엣지케이스 처리` 가 중요합니다.
   - 이분탐색의 시작인덱스 값이 1이고, A[i] 값이 `A[i]  <= minNumOfLISLength[1]` 이라면, 이분탐색은 1번의 조건을 만족하는 k 값을 찾을 수 없게 됩니다. 이 때는 A[i] 값으로 만들 수 있는 LIS의 길이가 1이라는 것을 의미하므로, LIS[1] = A[i] 값이 됩니다.
   - 또 다른 방법은 이분탐색의 시작인덱스를 0으로 설정하는 것입니다. 대신, 이 때의 minNumOfLISLength[0] 의 값은 A[i]의 어떤 값보다 작은 값으로 설정해야 합니다.
4. 수열 A의 LIS 길이는 lengthOfLIS 배열의 최댓값이 됩니다. 혹은, minNumOfLISLength 배열이 갱신된 최대 인덱스 값입니다.

<br>

#### 구현 소스코드

이를 소스코드로 구현하면 다음과 같습니다.

[백준 12015번](https://www.acmicpc.net/problem/12015) 문제의 조건을 이용하여 정답을 출력하는 소스코드입니다.

배열 A[i]의 범위만 다른 [백준 12738번](https://www.acmicpc.net/problem/12738) 문제도 아래의 소스코드로 해결가능합니다.

```java
import java.io.*;
import java.util.*;

public class Main {
    private static int N;
    private static int[] minNumOfLISLength;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        N = Integer.parseInt(br.readLine());
        int[] A = new int[N + 1];
        int[] lengthOfLIS = new int[N + 1];
        minNumOfLISLength = new int[N + 1];
        StringTokenizer tokenizer = new StringTokenizer(br.readLine());
        for (int i = 1; i <= N; i++) {
            A[i] = Integer.parseInt(tokenizer.nextToken());
        }

        minNumOfLISLength[0] = Integer.MIN_VALUE;
        for (int i = 1; i <= N; i++) {
            minNumOfLISLength[i] = Integer.MAX_VALUE;
        }

        for (int i = 1; i <= N; i++) {
            int k = binarySearch(A[i]);
            minNumOfLISLength[k + 1] = A[i];
            lengthOfLIS[i] = k + 1;
        }

        int answer = 1;
        for (int i = 0; i <= N; i++) {
            answer = Math.max(answer, lengthOfLIS[i]);
        }

        bw.append(answer + "");
        bw.flush();
    }

    private static int binarySearch(int num) {
        int start = 0;
        int end = N;
        while (start + 1 < end) {
            int mid = (start + end) / 2;
            if (minNumOfLISLength[mid] < num && num <= minNumOfLISLength[mid + 1]) {
                return mid;
            } else if (minNumOfLISLength[mid] >= num) {
                end = mid;
            } else {
                start = mid;
            }
        }
        return start;
    }
}
```



