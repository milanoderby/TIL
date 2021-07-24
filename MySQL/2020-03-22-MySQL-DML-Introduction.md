---
layout: post
title: "MySQL DML 입문"
date: 2020-03-22 19:00:00 +0900
categories: [mysql, database]
---



## MySQL DML 입문

1. select 문 (조회, read)

   ```mysql
   select [컬럼 명]
   from [테이블 명]
   where [조건]
   ```

   <br>

   - `as`:별칭(alias)

     - 컬럼 명에 AS명령어 적용: 결과 테이블의 컬럼 명에 별칭으로 출력된다. 

     ```mysql
     select user_id as id, user_name as name
     from user;
     ```

     - 테이블 명에 AS명령어 적용: 긴 테이블 명을 사용해야할 때, 별칭을 사용하여 테이블 명을 짧게 사용한다.

     ```mysql
     select E.employee_name, D.dept_name
     from employee as E, department as D;
     ```

     <br>

   - `distinct`: 명시한 컬럼에 대해서 중복을 제거하는 명령어

     ```mysql
     select distinct user_name, user_address
     from user;
     ```

     위의 쿼리 결과로 나온 결과 레코드에서 이름과 주소 2개의 값이 모두 같은 레코드들은(중복된 레코드) 제거하고 출력한다.

     결과 레코드의 결과로 user_name이 같지만, user_address가 다른 경우에는 레코드가 중복되지 않았으므로 출력한다.

     <br>

   - `distinctrow`: 조인할 때, 사용되나, 자주 사용되지 않는다.

     <br>

   - `order by`: 특정 컬럼에 대해서 정렬할 때, 사용

     ```mysql
     select [컬럼 명]
     from [테이블 명]
     ordery by [컬럼 명] [ASC | DESC](, [컬럼명] [ASC | DESC])
     ```

     - 오름차순은 `asc`, 내림차순은 `desc`으로 나타낸다.

     - 생략할 때, 기본 값은 오름차순으로 적용됨.

     - `asc` 와 `desc` 바로 앞 컬럼에 대해서 적용된다.
     - 일반적인 `order by`는 대/소문자를 구분하지 않는다.
     - 대/소문자를 구분하는 `order by`를 적용하려면, `order by binary`를 사용해야 한다.

     ```mysql
     mysql> SELECT name, species, birth FROM pet
            ORDER BY species, birth DESC;
     +----------+---------+------------+
     | name     | species | birth      |
     +----------+---------+------------+
     | Chirpy   | bird    | 1998-09-11 |
     | Whistler | bird    | 1997-12-09 |
     | Claws    | cat     | 1994-03-17 |
     | Fluffy   | cat     | 1993-02-04 |
     | Fang     | dog     | 1990-08-27 |
     | Bowser   | dog     | 1989-08-31 |
     | Buffy    | dog     | 1989-05-13 |
     | Puffball | hamster | 1999-03-30 |
     | Slim     | snake   | 1996-04-29 |
     +----------+---------+------------+
     // 위의 경우, birth에 대해서는 내림차순, species에 대해서는 오름차순으로 적용된다.
     ```

     <br>

   - `limit` : 결과 행의 출력 개수를 지정한다.

     ```mysql
     select *
     from user
     limit [offset], [row_count]
     ```

     - `offset+1`번째 행부터 `row_count`개의 행을 출력한다.

     - 생략하면, `offset`의 기본 값은 0, `row_count`의 기본 값은 1000이다.

       <br>

   - `group by` : 집단함수의 대상집단을 지정할 때, 사용되는 명령어

     - 집단함수에는 `avg` , `count` , `max` , `min` , `sum` 등이 있다.

     - 집단함수는 기본적으로 `NULL` 값을 무시하고 연산한다.

     - `count(*)`은 예외로 `NULL`값을 포함한 모든 튜플들의 개수를 출력한다.

     - `group by` 함수를 사용하지 않으면, 집단함수의 대상은 전체 행이 된다.

       ```mysql
       select avg(salary) as avgsal, max(salary) as maxsal
       from employee;
       +----------+---------+
       | avgsal   | maxsal  |
       +----------+---------+
       | 3500000  | 5000000 |
       +----------+---------+
       ```

     - `distinct` 절을 집단함수 내에서도 사용가능하다.

       ```mysql
       SELECT COUNT(DISTINCT student_name) FROM student;
       // 위 COUNT연산은 COUNT(*)과 다르게 NULL값이 아닌 student_name의 개수를 센다.
       // 집단함수 내의 컬럼 값들 중 중복된 값을 제거하고, 집단함수 연산을 실행한다.
       ```

       

     - `group by` 절에 사용된 컬럼이 동일한 값을 갖는 행(튜플)들이 각각 하나의 집단(그룹)으로 묶인다.

     - 각 그룹에 대해서 집단함수가 적용된다.

     - `group by`절로 그룹을 나눌 때, `select`절에는 `각 그룹 내에서 공통된 값을 가지는 컬럼`, `집단 함수`, `그룹화에 사용된 컬럼`들만 나타낼 수 있다.

       <br>

   - `having` : `group by`절로 나뉘어진 각 그룹이 만족해야하는 조건을 명시할 때, 사용되는 명령어

     - `having` 절에 사용되는 컬럼은 `각 그룹 내에서 공통된 값을 가지는 컬럼` 또는 `그룹화에 사용된 컬럼` 이거나, `집단함수에 포함`되어야 한다.

     - `where`절과 헷갈려서는 안된다. `having` 절은 `group by` 절로 나뉘어진 그룹에 대한 조건을 명시하는 것

     - ```mysql
       select dept_no, avg(salary) as avgsal, max(salary) as maxsal
       from employee
       group by dept_no
       having avg(salary) >= 2500000
       ```

       <br>

2. insert 문 (생성, create)

   ```mysql
   insert into [테이블 명]
   values (값1, 값2, 값3, 값4,...)
   // 위의 방식은 테이블에서 정의한 컬럼의 순서를 지켜서 값을 입력해야 한다.
   
   insert into [테이블 명] (컬럼1, 컬럼2, 컬럼3, 컬럼4,...)
   values (값1, 값2, 값3, 값4,...)
   // 위의 방식은 insert하고 싶은 컬럼의 값을 선택적으로 입력하면 된다.
   // 대신, 컬럼3, 컬럼5에만 값을 넣을 때, 도메인 조건에 맞는 값을 입력해야 한다.
   ```

   <br>

3. update 문 (수정, update)

   ```mysql
   update [테이블 명]
   set [컬럼 명] = (값), [컬럼 명] = (값),...
   where [조건]
   ```

   <br>

4. delete 문 (삭제, delete)

   ```mysql
   delete from [테이블 명]
   where [조건]
   // 참조되는 테이블의 삭제 연산의 결과로 참조 무결성 제약조건이 위배될 수 있으나,
   // 참조하는 테이블의 튜플을 삭제하면, 참조 무결성 제약조건을 위배하지 않음
   ```

   <br>

참조문서

- `select문`: [MySQL 8.0 Docs SELECT Statement](https://dev.mysql.com/doc/refman/8.0/en/select.html)
- `order by`절: [MySQL 8.0 Docs Sorting Rows](https://dev.mysql.com/doc/refman/8.0/en/sorting-rows.html)

