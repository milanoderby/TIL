### Item 6 - 불필요한 객체 생성을 피하라

한번 생성된 String 객체는 JVM 내에서 같은 것을 참조할 수 있도록 하자.

``` java
String str = new String("newString");
// 위 대신 아래 구문을 사용하자!
String str = "newString";
```
<br>
<br>
생성자 대신 정적 팩토리 메소드를 제공하는 불변 클래스에서는 정적 팩토리 메소드를 사용하여 불필요한 객체생성을 피할 수 있도록 하자.

``` java
Boolean b = new Boolean("true");
// 위 대신 아래 구문을 사용하자!
Boolean b = Boolean.valueOf("true");
```
<br>
<br>
값 비싼 객체를 내부적으로 생성하는 메소드는 값 비싼 객체를 불변 객체로 생성하여 성능향상을 꾀할 수 있다.

``` java
static boolean isRoman~(String s) {
	return s.matches("xxxxxxx");
}
// 위 대신 아래 구문을 사용하자!
private static final Pattern ROMAN = Pattern.compile("xxxxxxx");
static boolean isRoman~(String s) {
	return ROMAN.matcher(s).matches();
}
```