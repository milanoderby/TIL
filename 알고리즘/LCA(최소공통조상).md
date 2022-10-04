# LCA(최소공통조상)

### 정의

루트가 있는 트리(rooted tree)가 주어지고, 그 트리 상의 두 정점이 주어질 때 그들의 가장 가까운 공통 조상(Nearest Common Anscestor)은 다음과 같이 정의됩니다.

- 두 노드의 가장 가까운 공통 조상은, 두 노드를 모두 자손으로 가지면서 깊이가 가장 깊은(즉 두 노드에 가장 가까운) 노드를 말합니다.

<br>

![image-20221005004052564](./images/LCA-1)

예를 들어 15와 11를 모두 자손으로 갖는 노드는 4와 8이 있지만, 그 중 깊이가 가장 깊은(15와 11에 가장 가까운) 노드는 4 이므로 가장 가까운 공통 조상은 4가 됩니다.

루트가 있는 트리가 주어지고, 두 노드가 주어질 때 그 두 노드의 가장 가까운 공통 조상을 찾는 프로그램을 작성하세요

<br>

### 일반적인 방법으로 구하기

1. 각각의 노드의 연결정보를 나타내는 인접리스트를 생성합니다.
2. 루트노드와 1번에서 생성한 인접리스트를 이용하여 각각의 노드의 parent 배열을 생성합니다.
3. 루트노드와 1번에서 생성한 인접리스트를 이용하여 각각의 노드의 depth 배열을 생성합니다.
4. depth 배열과 parent 배열을 이용하여 주어진 2개 노드의 depth를 맞춰줍니다.
   1. depth가 동일해질 때까지 4-2번 작업을 반복합니다.
   2. 2개 노드 중 depth가 더 깊은 노드를 parent 배열로 갱신하여 노드의 depth를 낮춰줍니다.
   3. depth가 동일할 때, 2개의 노드가 동일하다면, 바로 최소공통조상을 찾은 것이므로 종료합니다.
   4. depth가 동일할 때, 2개의 노드가 동일하지 않다면, 최소공통조상을 찾지 못한 것이므로 5번 작업을 수행합니다.
5. depth가 동일해진 2개의 노드를 parent 배열을 이용하여 depth를 낮춰줍니다.
   1. 2개의 노드가 동일해질 때까지 각 노드를 노드의 parent로 갱신합니다.
   2. 2개의 노드가 동일하다면, 바로 최소공통조상을 찾은 것이므로 종료합니다.
   3. 2개의 노드가 동일하지 않다면, 최소공통조상을 찾지 못한 것이므로 5-1번 작업을 반복합니다.

<br>

#### 일반적인 방법의 시간복잡도

노드가 총 N개 일 때, 시간복잡도는 O(N / 2), 즉 O(N) 입니다.

가장 최악의 경우는 주어진 2개의 노드가 서로 트리에서 가장 멀리 떨어진 노드일 때입니다.

중요한 것은 이렇게 2개의 노드가 주어지는 사건이 M번 발생하는 문제가 주어지면, 시간복잡도는 O(N * M)이 됩니다.

<br>

### Sparse Table을 이용한 방법

시간복잡도를 줄이기 위해서 parent 배열을 sparse table로 구현하여 사용합니다.

쉽게 말하면, parent 배열을 이용하여 depth를 맞출 때, O(N) 이 아닌 O(log N) 이 소요되도록 sparse table을 만들겠다는 것입니다.

기존 parent 배열은 1차원 배열로 바로 위의 부모를 가리키는 배열인 반면,

sparse table로 구현한 parent 배열은 2차원 배열로 다음과 같이 정의됩니다.

paren\[k][n]: 노드 n의 2^k 부모를 가리킵니다.

<br>

depth의 차이를 diff라는 숫자라고 가정할 때, diff 는 2^p(0<= p <= log2(N))의 조합으로 구현할 수 있습니다.

그렇기 때문에 depth의 차이를 구하는 시간복잡도는 log(N) 으로 줄어들 게 됩니다.

또한, depth가 동일한 노드 2개의 depth를 낮춰가며 LCA를 찾을 때에도 sparse table로 구현한 parent 배열을 이용할 수 있습니다.

<br>

#### Sparse Table을 이용한 방법의 시간복잡도

이제 위의 방법을 이용하면, 2개 노드의 depth차이를 맞출 때, O(log N) 의 시간복잡도로 걸리게 됩니다.

이렇게 2개의 노드가 주어지는 사건이 M번 발생하는 문제가 주어지면, 시간복잡도는 O((log N) * M)이 됩니다.

<br>

### 소스코드

다음은 [백준 11438번](https://www.acmicpc.net/problem/11438) 문제를 풀면서, Sparse Table을 이용하여 LCA를 구하는 문제의 소스코드를 살펴보겠습니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        List<Integer>[] tree = new List[N + 1];
        for (int i = 1; i <= N; i++) {
            tree[i] = new ArrayList<>();
        }

        for (int i = 0; i < N - 1; i++) {
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(tokenizer.nextToken());
            int node2 = Integer.parseInt(tokenizer.nextToken());

            tree[node1].add(node2);
            tree[node2].add(node1);
        }

        int maxBitOfN = Integer.toBinaryString(N).length();
        int[][] parent = new int[maxBitOfN][N + 1];

        int rootNode = 1;

        boolean[] isVisited = new boolean[N + 1];
        isVisited[rootNode] = true;

        Queue<Integer> queue = new ArrayDeque<>();
        queue.add(rootNode);

        int[] depth = new int[N + 1];
        depth[0] = -1;
        depth[rootNode] = 0;

        parent[0][rootNode] = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (Integer next : tree[cur]) {
                if (isVisited[next]) {
                    continue;
                }
                isVisited[next] = true;
                queue.add(next);
                depth[next] = depth[cur] + 1;
                parent[0][next] = cur;
            }
        }

        for (int i = 1; i < maxBitOfN; i++) {
            for (int j = 1; j <= N; j++) {
                int temp = parent[i - 1][j];
                parent[i][j] = parent[i - 1][temp];
            }
        }

        int M = Integer.parseInt(br.readLine());
        for (int i = 0; i < M; i++) {
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(tokenizer.nextToken());
            int node2 = Integer.parseInt(tokenizer.nextToken());

            // 노드의 깊이를 맞추는 작업
            int deepNode = depth[node1] > depth[node2] ? node1 : node2;
            int shallowNode = depth[node1] > depth[node2] ? node2 : node1;

            for (int j = maxBitOfN - 1; j >= 0; j--) {
                int ancestor = parent[j][deepNode];
                if (depth[ancestor] >= depth[shallowNode]) {
                    deepNode = ancestor;
                }
            }

            int lca = deepNode;
            if (deepNode != shallowNode) {
                for (int j = maxBitOfN - 1; j >= 0; j--) {
                    int ancestorOfDeepNode = parent[j][deepNode];
                    int ancestorOfShallowNode = parent[j][shallowNode];
                    if (ancestorOfDeepNode != ancestorOfShallowNode) {
                        deepNode = ancestorOfDeepNode;
                        shallowNode = ancestorOfShallowNode;
                    }
                }

                lca = parent[0][deepNode];
            }
            bw.write(lca + "\n");
        }
        bw.flush();
    }
}
```