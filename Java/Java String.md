# Java String 

### String 생성방법에 따른 생성영역차이

String은 일반적인 객체와 다르게 new 연산자를 사용하지 않고도 인스턴스를 생성가능합니다. 그래서, 아래와 같이 2가지 방식으로 인스턴스를 생성할 수 있습니다.

<br>

#### String str = "hello";

이 방식으로 생성 시, 상수영역에 생성됩니다. 이 상수영역을 **String Pool 이라고 부르며, JVM에 문자열들을 저장하는 특별한 메모리 영역**입니다.

또한, 이렇게 생성 시, 기존에 상수영역에 **같은 문자열을 가지는 인스턴스**가 있는지 찾습니다. 만약 없다면, 상수영역에 생성하고, 있다면, 새로 생성되는 인스턴스는 이미 존재하는 인스턴스를 가리키게 됩니다.

<br>

#### String str = new String("hello");

이 방식으로 생성 시, 힙영역에 새로 생성됩니다. (위의 방식과 다르게 무조건, 새로 힙영역에 생성합니다.)

<br>

#### 예시 그림

![img](./images/java-string-1)

<br>

#### 예시 소스코드

```java
public static void main(String[] args) throws  IOException{
    String str1 = "hello";
    String str2 = "hello";
    System.out.println(str1 == str2);

    String str3 = new String("hello");
    String str4 = new String("hello");
    System.out.println(str3 == str4);
    
    // 각각 상수영역과 힙영역에 생성된 인스턴스이므로 false를 출력합니다.
    System.out.println(str1 == str3);
}

// 출력결과
true
false
false
```

<br>

### 한번 생성된 값은 변하지 않습니다. 즉, 불변객체입니다.

String 객체는 사용할 수 있는 메소드가 많고, 이 중에는 **문자열 조작 메소드**도 있습니다.

하지만, 이들 메소드 모두 **조작된 문자열을 반환하는 방식**으로 구현되어있으며, 객체인 문자열 자체는 절대 변하지 않게되어있습니다.

<br>

###### JVM에서 String Pool 안에 오직 하나의 동일한 문자열 사본만 저장하여 메모리 할당을 최적화하는 것을 *intern* 이라고 합니다.

**Intern** 이라는 영단어에는 구금하다, 억류하다라는 의미가 있습니다.

<br>

불변객체로 만들어서 얻는 효과로는 **보안, 동기화, 해시코드 캐싱, 성능향상** 등이 있습니다.

자세한 사항은 아래 참고자료를 확인합시다.

<br>

### 참고자료

https://programmers.co.kr/learn/courses/5/lessons/139#

https://www.baeldung.com/java-string-immutable