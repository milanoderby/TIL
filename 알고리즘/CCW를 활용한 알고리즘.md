# CCW를 활용한 알고리즘

### 사용시기

1. 세 점으로 이루어진 선분의 방향성을 구할 때 (시계방향인지, 반시계방향인지 판별)
2. 다각형의 넓이를 구할 때 (삼각형의 넓이 구하는 개념을 확장)
3. 2개의 선분이 교차하는지 파악할 때 (선분교차여부)

<br>

### 이론 - 벡터의 외적

CCW 알고리즘은 **벡터의 외적**을 기반으로 하는 알고리즘입니다.

벡터의 외적은 두 벡터를 곱하는 연산이며, 두 벡터를 외적한 결과는 단순 스칼라 값이 아닌, 방향을 포함한 벡터 값입니다. 다음은 벡터의 외적 계산공식입니다. (i, j, k는 각각 x축, y축, z축의 단위벡터입니다.)
$$
a=(x1,y1,z1),\,b=(x2,y2,z2)\,이고,\\
i=(1,0,0),\,j=(0,1,0),\,k=(0,0,1)\,일\,때\\
a×b=
\begin{bmatrix}
i & j & k\\
x1 & y1 & z1\\
x2 & y2 & z2
\end{bmatrix}\\
= (y1z2−y2z1,\,x2z1−x1z2,\,x1y2−x2y1)
$$
<br>

위와 같이 벡터의 외적은 3차원에서 통용되는 개념이지만, 각 점의 z좌표들을 0으로 고정시키면, 2차원 평면으로 가져올 수 있습니다. 위의 외적 계산식에서 a벡터와 b벡터의 z좌표 값을 0으로 바꾸면, 아래와 같은 결과를 얻을 수 있습니다.
$$
a=(x1,y1,0),\,b=(x2,y2,0)\,이고,\\
i=(1,0,0),\,j=(0,1,0),\,k=(0,0,1)\,일\,때\\
a×b=
\begin{bmatrix}
i & j & k\\
x1 & y1 & 0\\
x2 & y2 & 0
\end{bmatrix}\\
= (0,\,0,\,x1y2−x2y1)
$$
<br>

위를 통해 2차원 평면에서 **두 벡터의 외적연산을 한 결과벡터**는 다음의 특징을 가지는 것을 알 수 있습니다.
$$
방향:\,벡터\,a, b에 수직\\
크기:\,|a||b|sin\theta
$$
이 때, 정확한 방향은 오른손 법칙을 이용하여 구할 수 있습니다.

![img](./images/CCW-1)

a벡터와 b벡터가 이루는 방향성이 **반시계방향**이라면, 위를 가리키게 됩니다. 즉, **외적결과로 나온 벡터의 z좌표** **양수**가 됩니다.

a벡터와 b벡터가 이루는 방향성이 **시계방향**이라면, 위를 가리키게 됩니다. 즉, **외적결과로 나온 벡터의 z좌표**가 **음수**가 됩니다.

<br>

### 이론 - CCW

CCW는 **벡터 외적연산결과로 나온 벡터의 z좌표**를 이용하는 것입니다.

2차원 상의 두 벡터가 주어졌을 때, CCW 값은 다음과 같습니다.
$$
a=(x1,y1),\,b=(x2,y2)\,일\,때\\
CCW = x1y2−x2y1
$$
<br>

하지만, 보통 문제에서는 입력값으로 벡터를 그대로 주기보다는 벡터를 나타내는 **두 점을 잇는 선분** 형태로 주어집니다. 이 때, CCW 값은 다음과 같이 구할 수 있습니다.

벡터 a를 나타내는 선분AB와 벡터 b를 나타내는 선분CD가 있다고 할 때, CCW 값은 다음과 같습니다.
$$
A=(x1,y1),\,B=(x2,y2),\,C=(x3,y3),\,D=(x4,y4),\,일\,때\\
AB=(x2-x1, y2-y1),\,CD=(x4-x3, y4-y3)\\
CCW = (x2-x1)(y4-y3) − (y2-y1)(x4-x3)
$$
<br>

### CCW 활용

글 서두에 CCW를 이용하여 구할 수 있는 것 3가지에 대해 언급했습니다. 실제로 구하는 방법에 대해서 알아보겠습니다.

#### 1. 세 점으로 이루어진 선분의 방향성을 구할 때 (시계방향인지, 반시계방향인지 판별)

점 A, B, C가 주어졌을 때, 점 A, B, C를 순서대로 이은 선분의 방향이 시계방향인지 반시계방향인지 판별하기 위해서는 선분 AB와 선분 AC의 외적을 이용합니다.

![img](./images/CCW-1)

위 그림의 a벡터를 선분 AB로 보고, b벡터를 선분 AC로 보고 CCW 값을 계산합니다.
$$
A=(x1,y1),\,B=(x2,y2),\,C=(x3,y3)\,일\,때\\
AB=(x2-x1, y2-y1),\,AC=(x3-x1, y3-y1)\\
CCW = (x2-x1)(y3-y1) − (y2-y1)(x3-x1)
$$
<br>

CCW 값에 따른 선분 ABC의 방향성

1. 0일 때: 선분 AB와 선분 AC가 평행하다는 의미이며, 결국 점 A, B, C 모두 일직선 위에 있음을 나타냅니다.
2. 양수일 때: 반시계방향
3. 음수일 때: 시계방향

<br>

##### 구현 소스코드

[백준 11758번](https://www.acmicpc.net/problem/11758) 문제를 예시로 구현 소스코드까지 보겠습니다.

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        Point[] points = new Point[3];
        for (int i = 0; i < 3; i++) {
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            int x = Integer.parseInt(tokenizer.nextToken());
            int y = Integer.parseInt(tokenizer.nextToken());

            points[i] = new Point(x, y);
        }

        long ccw = ccw(points[0], points[1], points[2]);
        ccw = convertToSmallNum(ccw);
        bw.append(ccw + "");

        bw.flush();
    }

    private static long ccw(Point a, Point b, Point c) {
        return (long) (b.x - a.x) * (c.y - a.y) - (long) (b.y - a.y) * (c.x - a.x);
    }

    private static long convertToSmallNum(long number) {
        if (number == 0) {
            return 0;
        }
        return number > 0 ? 1 : -1;
    }

    private static class Point implements Comparable<Point> {
        private int x;
        private int y;

        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }

        @Override
        public int compareTo(Point p) {
            int compareByX = Integer.compare(x, p.x);
            if (compareByX != 0) {
                return compareByX;
            }

            return Integer.compare(y, p.y);
        }
    }
}
```

<br>

#### 2. 다각형의 넓이를 구할 때 (삼각형의 넓이 구하는 개념을 확장)

 2차원 평면 다각형을 이루는 점 N개가 순서대로 주어졌을 때, 다각형의 면적을 구하는 방법입니다. 다각형이란 것은 결국 여러개의 삼각형으로 쪼갤 수 있습니다. 예를 들면, 사각형의 점 4개가 주어졌을 때, **1번 점, 2번 점, 3번 점**이 이루는 삼각형의 넓이와 **1번 점, 3번 점, 4번 점**이 이루는 삼각형의 넓이를 합치면 사각형의 넓이를 구할 수 있습니다. 이런 식으로 확장하면, 점 N개가 이루는 다각형의 넓이를 구할 수 있습니다.

 그리고, 위의 **벡터 외적 이론**에서 설명했듯이 점 3개의 CCW를 이용하면, 삼각형의 넓이를 구할 수 있습니다. **벡터의 외적의 크기**는 **두 벡터가 이루는 삼각형의 넓이의 2배**이기 때문입니다. 

 위의 2가지를 이용하여 다각형을 이루는 삼각형의 넓이(**CCW의 1/2**)를 모두 더하고, 구한 면적에 절댓값을 씌워주면, 넓이를 구할 수 있습니다. 여기서, 면적에 절댓값을 취해주는 이유는 다각형의 모양에 따라 넓이가 음수로 나올 수 있기 때문입니다. 

##### 구현 소스코드

[백준 2166번](https://www.acmicpc.net/problem/2166)문제를 예시로 구현 소스코드까지 보겠습니다.

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());
        Point[] points = new Point[N];
        for (int i = 0; i < N; i++) {
            StringTokenizer tokenizer = new StringTokenizer(br.readLine());
            int x = Integer.parseInt(tokenizer.nextToken());
            int y = Integer.parseInt(tokenizer.nextToken());

            points[i] = new Point(x, y);
        }

        long dimension = 0;
        for (int i = 1; i + 1 < N; i++) {
            long ccw = ccw(points[0], points[i], points[i + 1]);
            dimension += ccw;
        }
        dimension = Math.abs(dimension);

        double answer = (double) dimension / 2;
        answer *= 10;
        answer = Math.round(answer);
        answer /= 10;

        bw.append(String.format("%.1f", answer));
        bw.flush();
    }

    private static long ccw(Point a, Point b, Point c) {
        return (long) (b.x - a.x) * (c.y - a.y) - (long) (b.y - a.y) * (c.x - a.x);
    }

    private static class Point implements Comparable<Point> {
        private int x;
        private int y;

        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }

        @Override
        public int compareTo(Point p) {
            int compareByX = Integer.compare(x, p.x);
            if (compareByX != 0) {
                return compareByX;
            }

            return Integer.compare(y, p.y);
        }
    }
}
```

<br>

다음은 넓이가 음수로 나올 때의 입력값입니다.

```
5
0 0
-2 0
1 1
0 4
-1 3
```

<br>

#### 3. 2개의 선분이 교차하는지 파악할 때 (선분교차여부)

선분교차를 판별하는 방법은 짧게 정리가 안되는 여러가지 경우의 수가 있어 다른 포스팅에 작성하겠습니다.

<br>

### 관련 문제

https://www.acmicpc.net/step/45

<br>

### 참고자료

https://ko.wikipedia.org/wiki/%EB%B2%A1%ED%84%B0%EA%B3%B1

https://wiki.mathnt.net/index.php?title=%EB%B2%A1%ED%84%B0%EC%9D%98_%EC%99%B8%EC%A0%81(cross_product)