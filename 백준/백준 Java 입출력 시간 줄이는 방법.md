# 백준 Java 입출력 시간 줄이는 방법

```java
public static void main(String[] args) throws IOException {
    // 버퍼 입력방식 이용
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    
    // 버퍼 입력으로 문자열을 받을 때
    String str = br.readLine();
    
    // 버퍼 입력으로 숫자를 받을 때
    int N = Integer.parseInt(br.readLine());
    
    // 버퍼 입력으로 공백으로 구분된 여러 개의 숫자를 받을 때
    StringTokenizer tokenizer = new StringTokenizer(br.readLine());
    for (int i = 0; i < N; i++) {
        int inputNum = Integer.parseInt(tokenizer.nextToken());
    }
    
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    bw.append("한 줄 출력\n");
    bw.append("라인 피드 안하고 출력");
    
    // 버퍼 출력이 실제로 발생하는 메소드
    bw.flush();
}
```

