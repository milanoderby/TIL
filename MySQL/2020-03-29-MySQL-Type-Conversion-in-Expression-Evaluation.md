---
layout: post
title: "MySQL 연산 시, 형변환 규칙"
date: 2020-03-29 14:15:00 +0900
categories: [mysql, database]
---



## MySQL 연산 시, 형변환 규칙

1. 암시적 형변환
   
   - MySQL이 자동으로 string 값을 number 값으로 변환하기도 하고, 역으로 변환하기도 한다.
   
     ```mysql
     mysql> SELECT 1+'1';
             -> 2
     mysql> SELECT CONCAT(2,' test');
             -> '2 test'
     ```
   
     <br>
   
2. 명시적 형변환

   - `CAST` 함수를 이용한다. - `CAST`함수는 `string`값을 반환한다.

     ```mysql
     mysql> SELECT 38.8, CAST(38.8 AS CHAR);
             -> 38.8, '38.8'
     mysql> SELECT 38.8, CONCAT(38.8);
             -> 38.8, '38.8'
     ```

<br>


3. 비교 연산시, 형변환 규칙
   - 양쪽의 인자들 중 하나라도 `NULL`값이라면, 비교연산의 결과는 `NULL`이다.
   - 예외적으로, `<=>`(NULL-safe 동등연산)은 `NULL`값들의 비교연산이 가능하다.
   - 양쪽의 인자들의 형태가 string이라면 string으로 비교가 된다.
   - 양쪽의 인자들의 형태가 integer이라면 integer으로 비교가 된다.
   - 16진수(Hexadecimal) 값은 숫자와의 비교연산이 이루어지지 않는다면, binary string으로 취급된다.
   - 양쪽의 인자 중 하나가 `timestamp` 또는 `datetime` 컬럼이고, 다른 한 쪽의 인자가 상수라면, 비교연산을 하기 전에 상수는 `timestamp`로 변환이된다. 예외적으로, 다른 한쪽의 인자가 `IN()` 함수의 내부 인자라면, 변환은 발생하지 않는다. 하지만, 이러한 자동형변환에 맡기는 것은 안전하지 않으므로 `CAST()` 함수를 이용하여`datetime` string 형태를 반드시 사용한다.
   - `Subquery`에서 반환하는 하나의 행은 상수로 취급되지 않는다. 그래서, 원하는 형태로 `CAST()`를 이용하여 형변환을 해준다.
   - 양쪽의 인자 중 하나가 10진수(`Decimal`)값이라면, 비교연산은 다른 한 쪽의 인자에 따른다. 예를 들어, 다른 한쪽이 `decimal`, `integer` 값이라면, 두 인자들은 10진수(`decimal`) 비교된다. 다른 한 쪽이 `부동소수점` 값이라면, `부동소수점` 비교로 진행된다.
   - 그 외 모든 케이스에 대해서 인자들은 `부동소수점` 비교로 진행된다. 예를 들어, 문자열(`string`) 과 숫자(`numeric`) 의 비교는 `부동소수점 숫자 비교`가 발생한다.

<br>

참조문서 [MySQL 8.0 Docs Type Conversion in Expression Evaluation](https://dev.mysql.com/doc/refman/8.0/en/type-conversion.html)