# Java Integer 클래스

primitive type 데이터인 int를 객체(= object)로 감싸주는 클래스입니다.

int형 데이터를 다룰 때 유용한 다양한 메소드와 필드를 제공합니다.

이 메소드에는 String과 관련한 연산, Bit와 관련한 연산을 수행하는 메소드도 포함합니다.

이 글에서는 적절한 분류 및 중요도 순으로 Integer 클래스를 알아봅니다.

<br>

## Field

| 수정자 및 반환타입 | 필드명    | 설명                                                         |
| ------------------ | --------- | ------------------------------------------------------------ |
| static int         | MAX_VALUE | int로 표현할 수 있는 가장 큰 수를 반환합니다.<br />2^31 - 1을 반환합니다. |
| static int         | MIN_VALUE | int로 표현할 수 있는 가장 작은 수를 반환합니다.<br />-2^31을 반환합니다. |
| static int         | SIZE      | int로 표현할 때, 필요한 비트의 개수들을 반환합니다.<br />32를 반환합니다. |

<br>

```java
public static void main(String[] args) throws IOException {
	System.out.println(decodeString("3[a]2[bc]"));

	System.out.println(Integer.MAX_VALUE);
	System.out.println(Integer.MIN_VALUE);
	System.out.println(Integer.SIZE);
}

// 출력결과
2147483647
-2147483648
32
```

<br>

## Method

### Method - 비교 관련

| 수정자 및 반환타입 | 메소드명                 | 설명                                                         |
| ------------------ | ------------------------ | ------------------------------------------------------------ |
| static int         | compare(int x, int y)    | 인자로 받은 두 수의 대소비교를 수행합니다.<br />x가 y보다 크면 1<br />x와 y가 같으면 0<br />x가 y보다 작으면 -1<br />조금 쉽게 생각하면, (x - y)의 부호를 따라간다고 생각하면 쉽습니다. |
| int                | compareTo(Integer other) | 인자로 받은 수와의 대소비교를 수행합니다.<br />객체의 값이 other객체의 값보다 크면 1<br />객체의 값이 other객체의 값이 같으면 0<br />객체의 값이 other객체의 값보다 작으면 -1<br />조금 쉽게 생각하면, (x - y)의 부호를 따라간다고 생각하면 쉽습니다. |

<br>

#### Integer.compare(int x, int y) 예제

```java
public static void main(String[] args) throws IOException {
	System.out.println(Integer.compare(10, 1));
    System.out.println(Integer.compare(10, 10));
    System.out.println(Integer.compare(10, 100));
}

// 출력결과
1
0
-1
```

<br>

#### IntegerInstance.compare(Integer other) 예제

```java
public static void main(String[] args) throws IOException {
    Integer num = new Integer(10);
	Integer smallNum = new Integer(1);
    Integer sameNum = new Integer(10);
    Integer bigNum = new Integer(100);

    System.out.println(num.compareTo(smallNum));
    System.out.println(num.compareTo(sameNum));
    System.out.println(num.compareTo(bigNum));
}

// 출력결과
1
0
-1
```

<br>

### Method - Bit 관련

| 수정자 및 반환타입 | 메소드명                         | 설명                                                         |
| ------------------ | -------------------------------- | ------------------------------------------------------------ |
| static int         | bitCount(int i)                  | 인수로 받은 i라는 숫자를 2의 보수 표현법으로 나타냈을 때, 1의 개수를 반환합니다. |
| static int         | highestOneBit(int i)             | 인수로 받은 i라는 숫자를 2의 보수 표현법으로 나타냈을 때, **가장 왼쪽에 나타나는 1이 의미하는 값**을 반환합니다. |
| static int         | lowestOneBit(int i)              | 인수로 받은 i라는 숫자를 2의 보수 표현법으로 나타냈을 때, **가장 오른쪽에 나타나는 1이 의미하는 값**을 반환합니다. |
| static int         | numberOfLeadingZeros(int i)      | 인수로 받은 i라는 숫자를 2의 보수 표현법으로 나타냈을 때, **가장 왼쪽에 1이 나타나기 전에 나타나는 0의 개수**를 반환합니다. |
| static int         | numberOfTrailingZeros(int i)     | 인수로 받은 i라는 숫자를 2의 보수 표현법으로 나타냈을 때, **가장 오른쪽에 1이 나타나기 전에 나타나는 0의 개수**를 반환합니다. |
| static int         | reverse(int i)                   | 인수로 받은 i라는 숫자를 2의 보수 표현법을 거꾸로 한 숫자를 반환합니다. |
| static int         | reverseBytes(int i)              | 인수로 받은 i라는 숫자를 2의 보수 표현법을 **바이트 단위**로 거꾸로 한 숫자를 반환합니다. |
| static int         | rotateLeft(int i, int distance)  | 인수로 받은 i라는 숫자를 2의 보수 표현법에서 비트들을 distance 칸만큼 왼쪽으로 이동시킵니다. |
| static int         | rotateRight(int i, int distance) | 인수로 받은 i라는 숫자를 2의 보수 표현법에서 비트들을 distance 칸만큼 오른쪽으로 이동시킵니다. |
| static int         | signum(int i)                    | 인수로 받은 i라는 숫자의 signum function (부호함수) 연산 값을 반환합니다.<br />여기서, 부호함수란 <br />양수면 1<br />0이면 0<br />음수면 -1<br />을 반환하는 함수를 말합니다. |

<br>

#### Integer 클래스의 Bit 연산 예시

```java
public static void main(String[] args) throws IOException {
    int num = 275;

    // 275라는 10진수를 2의 보수표현법으로 나타내면 00000000 00000000 00000001 00010011입니다.
    System.out.println(Integer.toBinaryString(num));
    
    int byteReversedNum = Integer.reverse(num);
    // 00000000 00000000 00000001 00010011 를 바이트(= 8비트)단위로 거꾸로 하면,
    // 00010011 00000001 00000000 00000000입니다.
    System.out.println(Integer.toBinaryString(byteReversedNum);

    int bitReversedNum = Integer.reverse(num);
	// 00000000 00000000 00000001 00010011 를 비트 단위로 거꾸로 하면,
    // 11001000 10000000 00000000 00000000입니다.
    System.out.println(Integer.toBinaryString(bitReversedNum));

	// 11001000 10000000 00000000 00000000 를 비트 단위로 왼쪽으로 1칸씩 이동시키면,
	// 10010001 00000000 00000000 00000001 입니다.
	System.out.println(Integer.toBinaryString(Integer.rotateLeft(bitReversedNum, 1)));
                       
	// 00000000 00000000 00000001 00010011 를 비트 단위로 오른쪽으로 1칸씩 이동시키면, 
	// 10000000 00000000 00000000 10001001 입니다.
	System.out.println(Integer.toBinaryString(Integer.rotateRight(num, 1)));

    System.out.println();

    // 1의 개수는 4개입니다.
    System.out.println(Integer.bitCount(num));
    
    // 가장 왼쪽 1이 의미하는 값은 256입니다.
    System.out.println(Integer.highestOneBit(num));
    
    // 가장 오른쪽 1이 의미하는 값은 1입니다.
    System.out.println(Integer.lowestOneBit(num));
    
    // 가장 왼쪽에 1이 나타나기 전에 0은 23개 나타납니다.
    System.out.println(Integer.numberOfLeadingZeros(num));
    
    // 가장 오른쪽에 1이 나타나기 전에 0은 0개 나타납니다.
    System.out.println(Integer.numberOfTrailingZeros(num));
}

// 출력결과
100010011
11001000100000000000000000000000
11001000100000000000000000000000
10010001000000000000000000000001
10000000000000000000000010001001

4
256
1
23
0
```

<br>

### Method - String 형 변환 - 반환형이 Integer 또는 int

| 수정자 및 반환타입 | 메소드명                      | 설명                                                         |
| ------------------ | ----------------------------- | ------------------------------------------------------------ |
| static Integer     | valueOf(int i)                | 인수로 받은 i라는 숫자를 값으로 가지는 Integer 객체를 반환합니다. |
| static Integer     | valueOf(String s)             | 인수로 받은 s라는 **String을 부호를 가지는 10진수 값으로 해석하여 변환**하고, 이를 값으로 가지는 Integer 객체를 반환합니다. |
| static Integer     | valueOf(String s, int radix)  | 인수로 받은 s라는 **String을 부호를 가지는 radix 진수 값으로 해석하여 변환**하고, 이를 값으로 가지는 Integer 객체를 반환합니다. |
| static Integer     | decode(String s)              | 인수로 받은 s라는 **String을 아래의 규칙에 맞게 정수로 변환**하고, 이 값을 Integer 객체로 반환합니다.<br />인수로 받는 s가 적절한 정수 데이터로 해석되기 위해서는 아래와 같은 형태를 가져야합니다.<br />`Sign` `Radix specifier` `Digits`<br />1. Sign은 부호를 나타냅니다. + 또는 - 를 나타내며, 없을 수도 있습니다. 없을 때에 부호는 + 를 의미합니다.<br />2. Radis specifier는 진법을 나타냅니다.<br />`0x` 또는 `0X` 또는 `#`은 16진법을 의미합니다.<br />`0`은 8진법을 의미합니다.<br />없을 때에는 10진법을 의미합니다.<br />3. Digits는 말 그대로, 숫자를 나타냅니다.<br />다음의 경우, NumberFormatException을 던집니다.<br />1. Digits 부분이 양수가 아닐 때<br />2. String 인수 중간에 공백문자가 있을 때 |
| static int         | parseInt(String s)            | 인수로 받은 s라는 **String을 부호를 가지는 10진수 값으로 해석하여 변환**하고, 이 값을 int형 변수로 반환합니다. |
| static int         | parseInt(String s, int radix) | 인수로 받은 s라는 **String을 부호를 가지는 radix 진수 값으로 해석하여 변환**하고, 이 값을 int형 변수로 반환합니다. |

<br>

#### Integer.valueOf(String s) vs Integer.decode(String s)차이점

Integer.valueOf(String s) 메소드는 **인수로 받는 문자열을 10진법으로 해석**하고,

Integer.decode(String s) 메소드는 **인수로 받는 문자열을 규칙에 의해 8, 10, 16진법으로 해석**할 수 있습니다.

<br>

#### Integer.valueOf vs Integer.parseInt 차이점

Integer.valueOf 메소드는 **반환형이 Integer** 이고,

Integer.parseInt 메소드는 **반환형이 int** 입니다.

<br>

#### Integer.valueOf(int i) vs new Integer(int i) 차이점

생성자 방식과 정적메소드 방식이라는 차이점이 있습니다.

2가지 방식 중 정적메소드 방식이 자주 요청되는 값을 캐싱함으로써, 더 좋은 시간과 공간적인 성능을 낼 수 있기 때문에 사용하는 것을 권장합니다.

[참고자료](https://docs.oracle.com/javase/7/docs/api/java/lang/Integer.html#valueOf(int))

<br>

### Method - String 형 변환 - 반환형이 String

| 수정자 및 반환타입 | 메소드명                   | 설명 |
| ------------------ | -------------------------- | ---- |
| static String      | toBinaryString(int i)      |      |
| static String      | toOctalString(int i)       |      |
| static String      | toString(int i)            |      |
| static String      | toString(int i, int radix) |      |

<br>

#### Integer 클래스의 String 형 변환 연산 예시

```java
public static void main(String[] args) throws IOException {
    int num = 275;

    // Integer.toString(num)은 10진수를 그대로 문자열로 변환하는 연산입니다.
    // 사실, 아래 작업은 IntelliJ IDE에서 수행하면, toString메소드를 불필요하다고 알려줍니다.
    // 왜냐하면, 그냥 num을 출력하는 것과 같기 때문입니다.
    System.out.println(Integer.toString(num));
    
    // 위와 동일한 결과가 출력됩니다.
    System.out.println(num);

    // 275라는 10진수를 2진수로 나타내면 00000001 00010011입니다.
    System.out.println(Integer.toBinaryString(num));
    
    // 275라는 10진수를 4진수로 나타내면 0001 0103입니다.
    System.out.println(Integer.toString(num, 4));
    
    // 275라는 10진수를 8진수로 나타내면 423입니다.
    System.out.println(Integer.toOctalString(num));
    
    // 275라는 10진수를 16진수로 나타내면 113입니다.
    System.out.println(Integer.toHexString(num));
}

// 출력결과
275
275
100010011
10103
423
113
```

<br>

### Method - primitive type 형 변환 관련

| 수정자 및 반환타입 | 메소드명      | 설명                                                   |
| ------------------ | ------------- | ------------------------------------------------------ |
| byte               | byteValue()   | Integer 객체의 값을 byte 변수에 저장하여 반환합니다.   |
| short              | shortValue()  | Integer 객체의 값을 short 변수에 저장하여 반환합니다.  |
| int                | intValue()    | Integer 객체의 값을 int 변수에 저장하여 반환합니다.    |
| long               | longValue()   | Integer 객체의 값을 long 변수에 저장하여 반환합니다.   |
| float              | floatValue()  | Integer 객체의 값을 float 변수에 저장하여 반환합니다.  |
| double             | doubleValue() | Integer 객체의 값을 double 변수에 저장하여 반환합니다. |

<br>

### Method - system property 값 변환 관련

| 수정자 및 반환타입 | 메소드명                           | 설명                                                         |
| ------------------ | ---------------------------------- | ------------------------------------------------------------ |
| static Integer     | getInteger(String nm)              | 인수로 nm이라는 **시스템 속성(=system property)의 key를 나타내는 변수를** 받고, 이 key에 대응되는 시스템 속성값을 반환합니다.<br />여기서 시스템 속성값은 JVM 기동 시, 자동으로 설정되는 값들을 말하며, 아래와 같은 것들을 내포합니다.<br /> Java 정보, JVM 정보, class 경로 및 버전, library 경로, OS 정보, 파일 구분자, 경로 구분자, User 정보<br />다음의 경우 null 값을 반환합니다.<br />1. 시스템 속성 key에 대응되는 값이 없을 때<br />2. 시스템 속성 key값이 empty string이거나(= "") null 값일 때<br />3. 시스템 속성 값이 정확한 숫자형식이 아닐 때 |
| static Integer     | getInteger(String nm, int val)     | 인수로 nm이라는 **시스템 속성(=system property)의 key를 나타내는 변수를** 받고, 이 key에 대응되는 시스템 속성값을 반환합니다.<br />여기서 시스템 속성값은 JVM 기동 시, 자동으로 설정되는 값들을 말하며, 아래와 같은 것들을 내포합니다.<br /> Java 정보, JVM 정보, class 경로 및 버전, library 경로, OS 정보, 파일 구분자, 경로 구분자, User 정보<br />다음의 경우 2번째 인수로 받은 **val 값을 가지는 Integer 객체**를 반환합니다.<br />1. 시스템 속성 key에 대응되는 값이 없을 때<br />2. 시스템 속성 key값이 empty string(= "")이거나 null 값일 때<br />3. 시스템 속성 값이 정확한 숫자형식이 아닐 때 |
| static Integer     | getInteger(String nm, Integer val) | 인수로 nm이라는 **시스템 속성(=system property)의 key를 나타내는 변수를** 받고, 이 key에 대응되는 시스템 속성값을 반환합니다.<br />여기서 시스템 속성값은 JVM 기동 시, 자동으로 설정되는 값들을 말하며, 아래와 같은 것들을 내포합니다.<br /> Java 정보, JVM 정보, class 경로 및 버전, library 경로, OS 정보, 파일 구분자, 경로 구분자, User 정보<br /><br />시스템 속성값은 Integer.decode 메소드가 해석하는 방식대로 값 해석을 수행합니다.<br />다음의 경우 2번째 인수로 받은 **val 객체**를 반환합니다.<br />1. 시스템 속성 key에 대응되는 값이 없을 때<br />2. 시스템 속성 key값이 empty string(= "")이거나 null 값일 때<br />3. 시스템 속성 값이 정확한 숫자형식이 아닐 때 |

<br>

## 참고자료

https://docs.oracle.com/javase/7/docs/api/java/lang/Integer.html

https://www.geeksforgeeks.org/how-to-pass-integer-by-reference-in-java/

https://stackoverflow.com/questions/3330864/how-can-i-pass-an-integer-class-correctly-by-reference