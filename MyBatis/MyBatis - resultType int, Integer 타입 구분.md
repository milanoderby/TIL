### MyBatis - resultType int, Integer 타입 구분

1. int 타입으로 받아도 되는 경우

   `SELECT count(column) FROM EXAMPLE_TABLE`

   위 쿼리와 같이 결과 값이 반드시 숫자 값으로 받을 수 있는 경우에는 int 타입을 사용해도 됩니다.

2. Integer 타입으로 받아야 하는 경우

   `SELECT nullable_column FROM EXAMPLE_TABLE`

   위 쿼리와 같이 결과 값이 반드시 null 값이 들어올 수 있는 경우에는 Integer 타입을 사용해야 됩니다.

<br>