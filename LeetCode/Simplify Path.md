# Simplify Path

### 개념

Absolute Path가 입력으로 주어졌을 때, 이를 Canonical Path로 변환하는 문제입니다.

<br>

### 문제풀이

1. 절대경로에는 `/`, `//`, `///`와 같은 문자가 경로를 의미할 때 들어갈 수 있는데, 이를 `/`로 변환하여야 합니다.
2. `.` 은 현재 경로를 의미합니다.
3. `..` 은 현재 파일의 상위 경로를 의미합니다. 단, 상위 경로가 존재하지 않는 root 경로의 경우, 부모 경로도 root 경로를 의미합니다.
4. `/` 문자를 delilmeter로 하여 절대경로를 잘라 각각의 파일명을 추출합니다.
5. 파일명이 빈 문자열이거나, `.` 문자열일 경우, Stack에 넣지 않습니다.
6. 파일명이 `..` 문자열일 경우, 부모 경로를 가리키는 것이므로 Stack에서 pop합니다.
7. 그 외 파일명은 다음 경로를 의미하므로 Stack에 push합니다.

<br>

### 구현

```java
class Solution {
    public String simplifyPath(String path) {
        List<String> pathList = Arrays.stream(path.split("/"))
            .collect(Collectors.toList());

        Stack<String> pathStack = new Stack<>();
        for (String fileName : pathList) {
            switch (fileName) {
                case "":
                    break;
                case ".":
                    break;
                case "..":
                    if (!pathStack.isEmpty()) {
                        pathStack.pop();
                    }
                    break;
                default:
                    pathStack.push(fileName);
                    break;
            }
        }

        String answer = pathStack.stream()
            .collect(Collectors.joining("/"));
        return "/" + answer;
    }
}
```

