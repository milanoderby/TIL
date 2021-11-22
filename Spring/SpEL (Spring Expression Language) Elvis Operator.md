# SpEL (Spring Expression Language) Elvis Operator

### Elvis Operator

Groovy 언어에서 사용되는 문법입니다.

표기방법은 `?:` 이며, 삼항연산자 `?` 를 사용할 때, 변수가 반복되는 단점을 해결하기 위해 사용됩니다.

예를 들어, `?` 을 사용할 때는, 아래와 같이 name이 반복됩니다.

```java
String name = "Elvis Presley";
String displayName = name != null ? name : "Unknown";
```

<br>

반면, `?:`을 사용할 때는 아래와 같이 반복이 없습니다. 아래의 예제는 context가 없기 때문에 name이 null이 맞고 결국, default 값으로 설정한 'Unknown' 값이 name에 대입됩니다.

```java
ExpressionParser parser = new SpelExpressionParser();
String name = parser.parseExpression("null?:'Unknown'").getValue(String.class);
System.out.println(name);  // 'Unknown'
```

<br>

더 복잡한 예제로, 다음과 같이 context를 함께 사용합니다.

`Name` 프로퍼티가 있으므로 해당 프로퍼티로 name에 값이 대입됩니다.

```java
ExpressionParser parser = new SpelExpressionParser();

Inventor tesla = new Inventor("Nikola Tesla", "Serbian");
StandardEvaluationContext context = new StandardEvaluationContext(tesla);

String name = parser.parseExpression("Name?:'Elvis Presley'").getValue(context, String.class);

System.out.println(name); // Mike Tesla

tesla.setName(null);

name = parser.parseExpression("Name?:'Elvis Presley'").getValue(context, String.class);

System.out.println(name); // Elvis Presley
```

<br>

### 참고자료

https://docs.spring.io/spring-framework/docs/3.0.x/spring-framework-reference/html/expressions.html

https://stackoverflow.com/questions/3965446/how-to-read-system-environment-variable-in-spring-applicationcontext/18724139#18724139