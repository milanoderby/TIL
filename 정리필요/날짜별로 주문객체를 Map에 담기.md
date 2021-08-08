# 날짜별로 주문객체를 Map에 담기

MySQL에서는 이렇게 연산하는 것이 힘들다.

GroupBy를 이용하려고 했지만, Group 함수는 여러 개의 컬럼을 하나의 값으로 만들어주는 함수이지, List형태로 만들어주는 함수가 아니다.

https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html

```mysql
SELECT DATE(ordered_ymdt) as ordered_date, MAX(order_no)
FROM order_sheet
GROUP BY ordered_ymdt
```

<br>

MyBatis를 이용하면, 다음과 같은 방법으로 가능하댄다.

https://programmer.group/return-to-map-in-mybatis.html

<br>

그렇기 때문에 List 형태로 날짜와 주문객체를 가져온 후, 이를 프로그래밍 언어 단에서 Map으로 바꿔주는 것이 좋다.

<br>

Java에서 MultiValueMap는 없다.

Guava 또는 apache.commons의 객체를 이용해야한다.

또는, Map<LocalDate, List\<Order>> 객체를 이용할 수 있다.

https://stackoverflow.com/questions/4956844/hashmap-with-multiple-values-under-the-same-key

<br>

javascript 에서도 마찬가지

https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Keyed_collections

<br>

Typescript에서는 어떻게 구현했는지 알아보자.

코드

<br>

Vue에서는 Map을 어떻게 렌더링하는지 알아보자.

https://github.com/vuejs/vue/issues/6644

<br>