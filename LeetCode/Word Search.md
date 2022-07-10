# Word Search

### 개념

배열이 주어지고, 해당 배열에서 문자열을 찾는 문제입니다.

<br>

### 문제풀이

DFS를 사용하여 문제를 해결하여야합니다.

배열의 각 지점에서 주어진 문자열(Word)의 index의 값과 매칭된다면, 다시 재귀적으로 메소드를 호출하여 

배열의 다음 지점에서도 word의 다음 index와 매칭되는지 확인합니다.

<br>

### 구현

```java
	public static boolean exist(char[][] board, String word) {
        boolean[][] isVisited = new boolean[board.length][board[0].length];

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                if (board[i][j] == word.charAt(0)) {
                    isVisited[i][j] = true;
                    boolean isFound = findWord(board, isVisited, i, j, word, 1);
                    if (isFound) {
                        return true;
                    }
                    isVisited[i][j] = false;
                }
            }
        }

        return false;
    }

    private static boolean findWord(char[][] board, boolean[][] isVisited, int y, int x,
        String word,
        int nextIndex) {
        if (nextIndex >= word.length()) {
            return true;
        }

        int[] dy = {-1, 1, 0, 0};
        int[] dx = {0, 0, -1, 1};

        for (int dirIndex = 0; dirIndex < 4; dirIndex++) {
            int nextY = y + dy[dirIndex];
            int nextX = x + dx[dirIndex];

            if (!(0 <= nextY && nextY < board.length && 0 <= nextX
                && nextX < board[0].length)) {
                continue;
            }

            if (isVisited[nextY][nextX]) {
                continue;
            }

            if (board[nextY][nextX] != word.charAt(nextIndex)) {
                continue;
            }

            isVisited[nextY][nextX] = true;
            boolean isFound = findWord(board, isVisited, nextY, nextX, word, nextIndex + 1);
            if (isFound) {
                return true;
            }
            isVisited[nextY][nextX] = false;
        }

        return false;
    }
```

