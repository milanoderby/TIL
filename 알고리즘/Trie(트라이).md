# Trie(트라이)

탐색 트리의 일종입니다.

주로, 문자열을 저장 및 탐색할 때, 사용됩니다.

문자열 트라이의 특징은 특정 노드의 자식노드들은 공통 접두사를 공유한다는 것입니다.

다음은 Trie 자료구조를 통해 문자열을 저장할 때, 모습입니다.

![img](./images/trie-1)

<br>

다음은, 트라이로 풀 수 있는 간단한 문제입니다.

### 백준 14425번

1. 문자열 집합 S에 N개의 문자열을 저장합니다.
2. M개의 문자열 중에서 몇개의 문자열이 집합 S에 속하는지 구합니다.

```java
import java.io.*;
import java.util.*;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer tokenizer = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(tokenizer.nextToken());
        int M = Integer.parseInt(tokenizer.nextToken());

        Trie trie = new Trie();
        for (int i = 0; i < N; i++) {
            trie.insert(br.readLine());
        }

        int answer = 0;
        for (int i = 0; i < M; i++) {
            if (trie.query(br.readLine())) {
                answer++;
            }
        }
        bw.append(answer + "");
        bw.flush();
    }

    private static class Trie {
        private Node root;

        public Trie() {
            this.root = new Node();
        }

        public void insert(String word) {
            Node temp = root;
            for (int i = 0; i < word.length(); i++) {
                int alphabet = word.charAt(i) - 'a';
                if (Objects.isNull(temp.next[alphabet])) {
                    temp.next[alphabet] = new Node();
                }
                temp = temp.next[alphabet];

                if (i == word.length() - 1) {
                    temp.isFinished = true;
                }
            }
        }

        public boolean query(String word) {
            Node temp = root;
            for (int i = 0; i < word.length(); i++) {
                int alphabet = word.charAt(i) - 'a';
                if (Objects.isNull(temp.next[alphabet])) {
                    return false;
                }
                temp = temp.next[alphabet];

                if (i == word.length() - 1 && temp.isFinished) {
                    return true;
                }
            }
            return false;
        }
    }

    private static class Node {
        private Node[] next;
        private boolean isFinished;

        public Node() {
            this.next = new Node[26];
            this.isFinished = false;
        }
    }
}
```

