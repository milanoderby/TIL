# Java 소수점 N번째 자리까지 반올림

### Math.round(float, double x)

Math.round 함수는 반올림을 하는 함수입니다. 

하지만, 기본적으로 N번째 자리까지 반올림을 하기 위해서 받아야하는 인자 값인 N은 없습니다.

<br>

대신, 인자로 받은 float나 double 형 변수인자의 소수점 이하 자리를 없애는 반올림 작업을 취합니다.

예를 들면 다음과 같습니다.

```java
double PI = 3.141529173889;
System.out.print(Math.round(PI));

// 출력
// 3
```

<br>

그러나, 우리가 원하는 것은 `N번째 자리까지 반올림` 이기 때문에 한가지 꼼수를 부립니다.

예를 들어, 소수점 2번째자리까지 반올림한 값을 구하고 싶을 때, 다음과 같이 연산합니다.

```java
double PI = 3.141529173889;
System.out.print(Math.round(PI * 100) / 100.0);

// 출력
// 3.14
```

위와 같이 100 을 곱해준 값으로 반올림을 취한 뒤, 100.0 으로 나누어주면, 소수점 2번째자리까지 반올림한 결과값을 얻을 수 있습니다.

<br>

### String.format(format, args)

위와 같은 방식은 한가지 문제가 있습니다.

예를 들어, 답인 2를 `2.00` 으로 출력해야하는 문제가 있다고 합시다.

하지만, 위의 방식을 사용하고 마무리 짓는다면, 아래와 같이 잘못된 결과를 출력할 것입니다.

```java
double num = 2;
System.out.print(Math.round(num * 100) / 100.0);

// 출력
// 2.0
```

<br>

따라서, 위와 같은 문제를 해결하기 위하여 `String.format` 메소드를 이용하여 출력형식을 소수점 둘째자리까지 0으로 채우도록합니다.

```java
double num = 2;
System.out.print(String.format(".2f", Math.round(num * 100) / 100.0));

// 출력
// 2.00
```