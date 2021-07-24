---
layout: post
title: "MySQL DDL 입문"
date: 2020-03-22 19:50:00 +0900
categories: [mysql, database]
---



## MySQL DDL 입문

1. create 문: `table`, `domain`, `view`, `index` 를 생성할 수 있다.

- 문법

  ```mysql
  create table [테이블 명](
  	[컬럼1] [컬럼1 도메인 제약조건],
      [컬럼2] [컬럼2 도메인 제약조건],
      [컬럼3] [컬럼3 도메인 제약조건],
      
      [check 제약조건]
      
      [key 제약조건: primary, foreign 등]
  );
  ```

  <br>

- 사용 예시

  ```
  ex)
  create table user(
      user_id varchar(20),
      user_password varchar(20),
  	department_number varchar(20),
      
      //check 제약조건
      check user_id >= 0,
      
      //key 제약조건
      primary key (user_id),
      foreign key(department_number) references department(department_no);
  );
  ```

  <br>

2. alter 문: `table`의 구조를 변경할 수 있다.

- 컬럼 추가

  ```mysql
  alter table [테이블 명] add [column] [컬럼 명] [도메인 제약조건]
  ```

  <br>

- 컬럼 도메인 제약조건 수정

  ```mysql
  alter table [테이블 명] modify [column] [컬럼 명] [도메인 제약조건]
  ```

  <br>

- 컬럼 이름, 도메인 제약조건 변경

  ```mysql
  alter table [테이블 명] change [column] [기존 컬럼 명] [변경된 컬럼 명][도메인 제약조건]
  ```

  <br>

- 컬럼 삭제

  ```mysql
  alter table [테이블 명] drop [column] [컬럼 명]
  ```

  <br>

- primary key 제약조건 추가

  ```mysql
  alter table [테이블 명] add [constraint [제약조건 명]] primary key([컬럼 명])
  ```

  <br>

- foreign key 제약조건 추가

  ```mysql
  alter table [테이블 명] add [constraint [제약조건 명]] foreign key([컬럼 명]) references [테이블 명]([컬럼 명])
  ```

  <br>

- check 제약조건 추가

  ```mysql
  alter table [테이블 명] add check([제약조건 표현식])
  ```

  <br>

- order by 절 추가

  ```mysql
  alter table [테이블 명] order by [컬럼 명,...]
  ```

  <br>

- 테이블 이름 변경

  ```mysql
  alter table [기존 테이블 명] rename [변경된 테이블 명]
  ```

<br>

3. drop 문: `table`, `domain`, `view`, `index` 를 제거할 수 있다.

   ```mysql
   drop table user; //user table 제거
   ```

   