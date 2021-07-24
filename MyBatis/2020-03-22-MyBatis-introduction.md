---
layout: post
title: "MyBatis 입문"
date: 2020-03-22 15:15:00 +0900
categories: [mybatis]
---



## MyBatis 입문

### 1. 왜 MyBatis를 사용하는가?

- MyBatis를 사용함으로써, 기존에 사용되던 많은 JDBC bolierplate code(상용적으로 사용되는 표준 코드)를 제거할 수 있다.

- select문, insert문과 같은 간단한 mysql문을 실행이 목적임에도 불구하고, JDBC는 low-level API이기 때문에 다양한 DB와 관련된 operation(실행)을 해주어야 한다.

- 그 예시는 다음과 같다.

- Student 객체

  ```
  import java.util.Date;
  public class Student
  {
    private Integer studId;
    private String name;
    private String email;
    private Date dob;
    // setters and getters
  }
  ```

- 실제 Id를 이용하여 DB에서 Student를 가져오는 예제

  ```
  public Student findStudentById(int studId)
  {
    Student student = null;
    Connection conn = null;
    try{
  //obtain connection
      conn = getDatabaseConnection();
      String sql = "SELECT * FROM STUDENTS WHERE STUD_ID=?";
  //create PreparedStatement
      PreparedStatement pstmt = conn.prepareStatement(sql);
  //set input parameters
      pstmt.setInt(1, studId);
      ResultSet rs = pstmt.executeQuery();
  //fetch results from database and populate into Java objects
      if(rs.next())  {
        student = new Student();
        student.setStudId(rs.getInt("stud_id"));
        student.setName(rs.getString("name"));
        student.setEmail(rs.getString("email"));
        student.setDob(rs.getDate("dob"));
      }
    } catch (SQLException e){
      throw new RuntimeException(e);
    }finally{
  //close connection
      if(conn!= null){
        try {
          conn.close();
        } catch (SQLException e){ }
      }
    }
    return student;
  }
  public void createStudent(Student student)
  {
    Connection conn = null;
    try{
  //obtain connection    
      conn = getDatabaseConnection();
      String sql = "INSERT INTO STUDENTS(STUD_ID,NAME,EMAIL,DOB)  
      VALUES(?,?,?,?)";
  //create a PreparedStatement
      PreparedStatement pstmt = conn.prepareStatement(sql);
  //set input parameters
      pstmt.setInt(1, student.getStudId());
      pstmt.setString(2, student.getName());
      pstmt.setString(3, student.getEmail());
      pstmt.setDate(4, new  
      java.sql.Date(student.getDob().getTime()));
      pstmt.executeUpdate();
    } catch (SQLException e){
      throw new RuntimeException(e);
    }finally{
  //close connection
      if(conn!= null){
        try {
          conn.close();
        } catch (SQLException e){ }
      }
    }
  }
  protected Connection getDatabaseConnection() throws SQLException
  {
    try{
      Class.forName("com.mysql.jdbc.Driver");
      return DriverManager.getConnection 
      ("jdbc:mysql://localhost:3306/test", "root", "admin");
    } catch (SQLException e){
      throw e;
    } catch (Exception e){
      throw new RuntimeException(e);
    }
  }
  ```

- 기존의 JDBC를 사용하면 다음과 같은 과정을 매 SQL 코드 내부에서 실행해줘야 한다.
  1. 각각의 SQL 코드 내부에서 DB Connection을 생성
  2. SQL문을 작성
  3. Input parameter를 설정
  4. 할당된 자원(connecton, statement, result set 등)을 회수

<br>

- MyBatis는 개발자가 오직 SQL문 작성에 집중할 수 있도록, 위의 업무들을 추상화한다. (공통적으로 실행되도록 한다.) 또한, 다음의 과정을 자동으로 처리해준다.
  1. 자바 객체의 속성들을 쿼리문의 인자로 채워준다.
  2. 쿼리의 결과를 자바 객체로 맵핑한다.

<br>

### 2. MyBatis의 특징

- `Low learning curve`: Java와 SQL에 익숙하다면, 누구나 쉽게 적용가능하다.
- `Works well with legacy database`: full-fledged(완전히 발달한) ORM 프레임워크(Hinbernate와 같은)는 Java 객체와 DB 테이블을 정적으로 맵핑하기 때문에 legacy DB를 다루기에 적합하지 않다. 반면에 MyBatis는 Java 객체와 Query 결과를 맵핑함으로서 legacy DB를 다루는 것이 쉽다.
- `Embraces SQL`: Hibernate와 같은 full-fledged ORM 프레임워크는 SQL 쿼리를 보이지 않는 곳에서 생성한다. 즉, DB에 특화된 기능을 이용할 수가 없다. 반면, MyBatis는 SQL을 포함하기 때문에 DB 특화된 기능을 이용할 수 있고, 최적화된 SQL 쿼리를 준비할 수 있다. 또한, stored procedure(저장된 프로시져)도 지원한다.
- `Supports integration with Spring and Guice frameworks`: 인기있는 DI framework인 Spring과 Guice의 기본통합을 지원한다.
- `Supports integration with third-party cache libraries`: SqlSession level의 ResultSet 범위 내에서 MyBatis는 `caching`기능을 자체 지원한다. 또한, EHCache, OSCache, Hazelcast와 cache library를 통합지원한다.
- `Better performance`: software aplication 성공의 핵심  중 하나는 성능이다. 많은 어플리케이션에서persistence layer가 전반적인 시스템 성능의 key이다. 
  - `MyBatis는 DB connection pooling을 지원한다.` 그래서, 매 요청 때마다 db connection 생성을 해야하는 비용을 줄여준다.
  - `MyBatis는 내부적으로 cache 메커니즘이 존재한다.` 그래서, SqlSession level에서 SQL 쿼리결과들을 caching 해두었다가 나중에 같은 요청이 들어올 때, 그 결과를 반환한다.
  - `MyBatis는 proxying을 사용하지 않는다.` 그래서, proxying을 하는 다른 ORM 프레임워크에 비해 더 좋은 성능을 낼 수있다. 
- 만약, application이 object model에 의해 구동되고, 그로 인해 SQ을 동적으로 생성해야한다면, MyBatis는 그다지 좋은 선택이 아닐 것이다. 또한, `transitive persistence mechanism`을 이용해야하는 경우,(parent obejct를 저장할 때, 연관된 child object 또한 persist(유지)해야하는 경우,) Hibernate가 더 적합할 것이다. 

<br>

[참조문서: Why MyBatis](https://hub.packtpub.com/why-mybatis/)