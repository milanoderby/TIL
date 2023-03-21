# 알고리즘 문제해결전략 9장 문제 - 틱택토 (ID TICTACTOE)

### 문제링크: https://www.algospot.com/judge/problem/read/TICTACTOE

<br>

### 문제해석

1. 3 X 3 크기의 게임판이 있습니다.

2. 두 명이 번갈아가며 o와 x를 게임판의 빈 칸을 채웁니다.

3. 같은 글자 3개로 가로, 세로, 대각선 1줄을 채우는 쪽이 이깁니다.

4. 게임을 시작할 때 글자를 놓는 순서는 항상 x부터 시작합니다.


<br>

### 문제 풀이 - 동적계획법 (Top-Down 방식)

1. 게임판의 상태 board가 주어질 때, 현재 글자를 쓰는 player가 최선을 다할 때, 얻을 수 있는 최선의 결과를 구하는 메소드를 아래와 같이 정의합니다.
1. int canWin(char\[][] board, char player, int[] playerCanWin)
   - board: 게임판의 상태를 의미합니다.
   - player: 현재, 글자를 쓰는 선수를 의미합니다.
   - playerCanWin[boardStatus]: 게임판의 상태 boardStatus에 따라 주어진 player가 최선을 다할 때, 얻을 수 있는 최선의 결과를 저장하는 메모이제이션 변수입니다.
     - boardStatus: 현재 board의 상태를 정수상태로 변환하여 저장합니다. 9칸에 대해서 빈칸, 'x', 'o'로 채워질 수 있으므로 각 board의 상태를 정수로 변환하면 최대 3^9가지 경우의 수가 나옵니다.
   - 위 메소드를 구현하는 방법은 아래와 같습니다.
   - 현재 board에서 놓을 수 있는 모든 칸에 1칸을 현재 player의 글자로 놓고 turn을 넘겼을 때, 상대방이 얻을 수 있는 최선의 결과(= canWin(board, otherPlayer, playerCanWin))를 가져옵니다.
     - 각 결과가 반환하는 최선의 결과는 각각 다음과 같습니다.
     - 1: 이길 수 있음
     - 0: 비길 수 있음
     - -1: 질 수 밖에 없음
     - 각 칸에 놓았을 때의 결과가 위와 같다면, 현재 player가 최선의 결과를 얻기 위해서는 상대방이 얻을 수 있는 최선의 결과 숫자가 낮은 쪽으로 글자를 놓으면 됩니다.
     - 따라서, 각 결과 중 최솟값을 선택한다고 생각하면 됩니다.
     - 다만, 상대방의 결과가 -1이면, 본인의 결과는 1 / 상대방의 결과가 1이면, 본인의 결과는 -1이 될 것 입니다.
   - 첫 번째 기저조건은 상대방이 글자를 놓은 후, 게임판의 상태가 상대방이 이긴 것으로 판단되는 경우이며, 이 때는 -1을 반환합니다. (본인은 진 것이므로)
   - 두 번째 기저조건은 게임판에 글자를 놓을 빈칸이 없는 경우이며, 이 경우에는 0을 반환합니다. (게임의 결판을 못낸 것이므로 비긴 것)

<br>

### 소스코드

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int C = Integer.parseInt(br.readLine());
        for (int i= 0; i < C; i++) {
            char[][] board = new char[3][3];

            int countOfX = 0;
            int countOfO = 0;
            for (int j = 0; j < 3; j++) {
                String line = br.readLine();
                for (int k = 0; k < 3; k++) {
                    board[j][k] = line.charAt(k);
                    if (board[j][k] == 'x') {
                        countOfX++;
                    } else if (board[j][k] == 'o') {
                        countOfO++;
                    }
                }
            }

            char turnPlayer, otherPlayer;
            if (countOfX == countOfO) {
                turnPlayer = 'x';
                otherPlayer = 'o';
            } else {
                turnPlayer = 'o';
                otherPlayer = 'x';
            }

            int[] playerCanWin = new int[(int) Math.pow(3, 9)];
            for (int j = 0; j < (int) Math.pow(3, 9); j++) {
                playerCanWin[j] = -2;
            }

            int canWin = canWin(board, turnPlayer, playerCanWin);
            if (canWin == 1) {
                bw.write(turnPlayer + "\n");
            } else if (canWin == -1) {
                bw.write(otherPlayer + "\n");
            } else {
                bw.write("TIE\n");
            }
        }
        bw.flush();
    }

    private static int canWin(char[][] board, char player, int[] playerCanWin) {
        char otherPlayer = (char) ('x' + 'o' - player);
        int boardStatus = convertBoardStatusToIntValue(board);

        if (playerCanWin[boardStatus] != -2) {
            return playerCanWin[boardStatus];
        }
        
        if (judgePlayerWin(board, otherPlayer)) {
            return playerCanWin[boardStatus] = -1;
        }

        if (!canPut(board)) {
            return playerCanWin[boardStatus] = 0;
        }

        int worstResultOfOtherPlayer = 2;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == '.') {
                    board[i][j] = player;
                    worstResultOfOtherPlayer = Math.min(worstResultOfOtherPlayer, canWin(board, otherPlayer, playerCanWin));
                    board[i][j] = '.';
                }
            }
        }
        return playerCanWin[boardStatus] = -worstResultOfOtherPlayer;
    }

    private static int convertBoardStatusToIntValue(char[][] board) {
        int boardStatus = 0;
        int powerOfThree = 1;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                int player = 0;
                if (board[i][j] == 'x') {
                    player = 1;
                } else if (board[i][j] == 'o') {
                    player = 2;
                }
                boardStatus += powerOfThree * player;
                powerOfThree *= 3;
            }
        }
        return boardStatus;
    }

    private static boolean judgePlayerWin(char[][] board, char player) {
        for (int i = 0; i < 3; i++) {
            boolean playerCanWin = true;
            for (int j = 0; j < 3; j++) {
                if (board[i][j] != player) {
                    playerCanWin = false;
                    break;
                }
            }

            if (playerCanWin) {
                return true;
            }
        }

        for (int j = 0; j < 3; j++) {
            boolean playerCanWin = true;
            for (int i = 0; i < 3; i++) {
                if (board[i][j] != player) {
                    playerCanWin = false;
                    break;
                }
            }

            if (playerCanWin) {
                return true;
            }
        }

        boolean playerCanWin = true;
        for (int i = 0; i < 3; i++) {
            if (board[i][i] != player) {
                playerCanWin = false;
                break;
            }
        }

        if (playerCanWin) {
            return true;
        }

        playerCanWin = true;
        for (int i = 0; i < 3; i++) {
            if (board[i][2 - i] != player) {
                playerCanWin = false;
                break;
            }
        }

        if (playerCanWin) {
            return true;
        }
        return false;
    }

    private static boolean canPut(char[][] board) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[i][j] == '.') {
                    return true;
                }
            }
        }
        return false;
    }
}
```
