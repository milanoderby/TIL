# Oracle NVL2 함수

### 사용법

NVL2(expr1, expr2, expr3)

expr1 이 null이 아니면, expr2 를 반환합니다.

expr1 이 null이면, expr3 을 반환합니다.

<br>

### 활용

Oracle 에서 parameter 로 주어진 Boolean 값을 통해 특정 컬럼이 null 값인지 아닌지를 구분하기 위해서 다음과 같이 사용합니다.

1. isAnswered 값이 null 이면, 모든 행을 추출합니다.
2. isAnswered 값이 true 이면, `column` 이 null 인 행들을 추출합니다.
3. isAnswered 값이 false 이면, `column` 이 null 이 아닌 행들을 추출합니다.

아래는 Spring Data JDBC @Query 의 Where 조건절을 예시로 한 것 입니다.

```sql
where (:isAnswered IS NULL OR NVL2(column, 1, 0) = :isAnswered)
```

<br>

### 참고자료

https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions106.htm