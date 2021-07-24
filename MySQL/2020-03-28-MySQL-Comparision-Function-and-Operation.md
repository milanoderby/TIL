---
layout: post
title: "MySQL 비교함수 및 연산"
date: 2020-03-28 22:50:00 +0900
categories: [mysql, database]
---



## MySQL Comparision Function and Operation

- 비교연산은 `string` 과 `number` 인자 사이에서 사용되고, 결과는 1(`TRUE`) , 0(`FALSE`) , `NULL` 중 하나이다.

- `CAST()` 함수는 비교연산의 목적으로 값을 특정 type으로 바꿀 때, 사용된다.

- `CONVERT()` 함수는 `String` 값을 다른 characterset으로 바꿀 때, 사용된다. 기본적으로, 문자열비교는 대/소문자를 구분하지 않는다. (`character set`의 기본 값이 대/소문자를 구분하지 않는 `utf8mb4` 이기 때문에)

  <br>

- 다음의 연산은 스칼라(`scalar`) 인자 비교연산 뿐만 아니라, `row operand`에 대한 비교연산도 가능하다.

  ```mysql
  =  >  <  >=  <=  <>  !=
  ```

- `<>` 와 `!=` 모두 같지 않음을 의미한다.

- (a, b) <= (x, y) 의 `row comparision`은 다음을 의미한다.

  ```mysql
  (a < x) OR ((a = x) AND (b <= y))
  ```

  <br>

- `<=>` : `NULL-safe equal` 을 의미한다. 일반적인 `=`연산은 두 개의 값 중 하나라도 `NULL` 값을 포함한다면, `NULL` 값을 반환하게 되어있다. 하지만, `NULL-safe equal`연산자는 다음과 같이 연산한다.

  ```mysql
  mysql> SELECT 1 <=> 1, NULL <=> NULL, 1 <=> NULL;
          -> 1, 1, 0
  mysql> SELECT 1 = 1, NULL = NULL, 1 = NULL;
          -> 1, NULL, NULL
  ```

  <br>

- `between`

  - `between` 연산의 3개의 인자들이 모두 같은 `type`이라면, `operand` between `min` and `max`는 다음과 같은 연산이다.

    ```mysql
    min <= operand and operand <= max
    ```

  - 3개의 인자들 `type`이 다르다면, 형변환 규칙에 의해 형변환이 발생한다.

  - `between` 연산을 이용하여 날짜를 비교할 때에는 `CAST()` 함수를 이용하여 값을 적절히 바꾼다. `DATE` 형태를 `DATETIME`으로 바꾸거나, `2020-01-01`과 같은 상수 값을 `DATE`형으로 바꾼다.

    <br>

- `coalesce` : list의 값 중 가장 첫 번째 `non-NULL` 값을 반환한다.

  ```mysql
  mysql> SELECT COALESCE(NULL,1);
          -> 1
  mysql> SELECT COALESCE(NULL,NULL,NULL);
          -> NULL
  ```

  <br>

- `greatest` : list의 값 중 가장 큰 값을 반환한다.

  ```mysql
  mysql> SELECT GREATEST(2,0);
          -> 2
  mysql> SELECT GREATEST(34.0,3.0,5.0,767.0);
          -> 767.0
  mysql> SELECT GREATEST('B','A','C');
          -> 'C'
  ```

  - list 값 중 하나라도 `NULL` 값이라면 `NULL` 값을 반환한다.

    <br>

- `like`: 문자열을 비교할 때, 사용하는 명령

  - 비교하는 문자열 둘 중 하나라도 NULL이라면, 결과도 NULL을 반환한다.

  - `wildcard 문자`로 `%` 와 `_`을 사용한다.

  - `%`는 n개의 문자를 의미한다.(0 <= n <= 무한대)

  - `_`는 정확히 1개의 문자를 의미한다.

    ```mysql
    select *
    from user
    where user_name like '이%';
    ```

  - 쿼리 결과: '이'로 시작하는 이름을 가진 user의 정보를 모두 가져온다.

  - `wildcard 문자` 를 포함하는 문자열을 찾는 연산을 하고 싶을 때에는 `이스케이프 문자`를 사용한다.`이스케이프 문자`는 기본으로 `\`으로 사용한다.

  - 즉, `_`를 포함하는 문자열을 찾고 싶을 때에는 다음과 같은 명령어를 사용한다.

    ```mysql
    SELECT 'David!' LIKE 'David\_';
    -> 0
    SELECT 'David_' LIKE 'David\_';
    -> 1
    ```

  - `ESCAPE` 절을 이용하여 이스케이프 문자(1개의 문자)를 바꿀 수도 있다.

    ```mysql
    SELECT 'David_' LIKE 'David|_' ESCAPE '|';
    -> 1
    ```

  - 일반적으로 사용할 때에는 대/소문자를 구분하지 않으나, 연산이 되는 대상이 `case-sensitive collation` 이나 `binary string`을 사용할 때에는 대소문자를 구분한다.

    ```mysql
    SELECT 'abc' LIKE 'ABC';
    -> 1
    SELECT 'abc' LIKE _utf8mb4 'ABC' COLLATE utf8mb4_0900_as_cs;
    -> 0
    SELECT 'abc' LIKE _utf8mb4 'ABC' COLLATE utf8mb4_bin;
    -> 0
    SELECT 'abc' LIKE BINARY 'ABC';
    -> 0
    ```

  - 표준 SQL에서 확장하여 MySQL에서는 문자열이 아닌 숫자에 대해서도 like문을 적용할 수 있다.

    ```mysql
    SELECT 10 LIKE '1%';
    -> 1
    ```

    <br>

- `in`

  > `operand` IN (values, . . .)
  >
  > `operand` 값이 list에 있는 값들 중 하나와 일치한다면 , true를 반환한다. 아니라면, false 반환

  - list의 값들이 같은 type이 아니라면, MySQL 형변환 규칙에 따라 형변환이 발생한다. 그 이유는 `최적화과정`으로 `이진탐색을 위한 sorting`과정을 거치야 하기 때문이다.

  - list의 값이 형변환이 필요하지 않은 경우, 모두 동일한 유형의 JSON이 아닌 상수이다.

  - `in` 연산은 `row constructor`에 대해서도 연산가능하다.

    ```mysql
    mysql> SELECT (3,4) IN ((1,2), (3,4));
            -> 1
    mysql> SELECT (3,4) IN ((1,2), (3,5));
            -> 0
    ```

  - 암시적 형변환은 적절하지 않은 결과를 만들어낸다.

    ```mysql
    mysql> SELECT 'a' IN (0), 0 IN ('b');
            -> 1, 1
    ```

  - SQL 표준을 지키기 위해 다음의 상황에서 `NULL` 값을 출력한다.

    - `operand`가 `NULL` 일 때

    - list 의 값 중에`operand` 와 일치하는 값이 없고, list의 값 중 하나가 `NULL` 일 때

      <br>

- `any` , `some` 

  > `operand` `comparison_operator` ANY (*subquery*)

  - `subquery`의 결과 값들 중 하나라도 `operand`와 비교한 연산자의 결과 값이 true라면, true를 반환한다.

    ```mysql
    SELECT s1 FROM t1 WHERE s1 > ANY (SELECT s1 FROM t2);
    ```

  - 위의 연산에서, `s1`이 10이고, `subquery`의 결과 값이 21, 14, 7 이라면, s1은 조건에서 true를 반환한다.

  - `some`은 `any`와 같은 연산이지만, 영어식 표현에서 자연스러움을 위해 `some` 이라는 연산자가 만들어졌다.

  - `in`은 `= any`와 같은 연산이다.

    <br>

- `IS` , `IS NOT` , `IS NULL` , `IS NOT NULL` , `ISNULL(expr)`

  - `IS` 는 `boolean` 값에 대해 판단하여 1(`TRUE`) , 0(`FALSE`) , `NULL(= UNKNOWN)` 중 하나를 반환한다.

  - `IS NOT` 은 `IS`의 부정

    ```mysql
    mysql> SELECT 1 IS NOT UNKNOWN, NULL IS UNKNOWN;
            -> 1, 1
    ```

  - `IS NULL`  , `IS NOT NULL` : 값이 `NULL`인지를 판단하는 연산자

  - `ISNULL(expr)` : `expression`이 `NULL`인지를 판단하는 연산자

    ```mysql
    mysql> SELECT NULL IS NULL, NULL IS NOT NULL;
            -> 1, 0
    mysql> SELECT ISNULL(1+1);
            -> 0
    mysql> SELECT ISNULL(1/0);
            -> 1
    ```

    <br>

- `AND` , `OR` , `NOT` : 조건절의 부가적인 요소

  - `NOT` 연산은 연산자의 바로 앞에 사용된다. ex: `not like` , `not in`

  ```mysql
  select *
  from user
  where user_name like '최%' and user_address like '서울%'
  ```

<br>

참조문서: [MySQL 8.0 Docs String Comparision Function](https://dev.mysql.com/doc/refman/8.0/en/string-comparison-functions.html)