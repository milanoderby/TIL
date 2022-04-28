# Java char배열 - String 변환

### String 을 char 배열로 변환

String 의 인스턴스 메소드인 toCharArray() 메소드를 이용합니다.

```java
String str = "dcba";
char[] charArray = str.toCharArray();
```

<br>

### char 배열을 String 으로 변환

String 의 static 메소드인 valueOf(char[] inputCharArray) 메소드를 이용합니다.

```java
String str = "dcba";
char[] charArray = str.toCharArray();
Arrays.sort(charArray);
String convertedString = String.valueOf(charArray);
System.out.println(convertedString);

// 출력결과
abcd
```

사실 String.valueOf() 내부구현을 살펴보면 `return new String(charArray);` 를 반환하는 것을 알 수 있습니다. 다만, staticMethod 를 사용하여 의미를 가지도록 한 것이 특징입니다.

<br>

#### 헷갈리는 메소드

Arrays.toString(char[] inputCharArray) 를 사용하면, 우리가 원하는 깔끔한 상태의 String이 아닌 `,`로 구분이 되어있는 char 배열을 결과로 얻게 됩니다.

```java
String str = "dcba";
char[] charArray = str.toCharArray();
Arrays.sort(charArray);
String unwantedString = Arrays.toString(charArray);
System.out.println(unwantedString);

// 출력결과
[a, b, c, d]
```

<br>

### 참고자료

https://stackoverflow.com/questions/7655127/how-to-convert-a-char-array-back-to-a-string