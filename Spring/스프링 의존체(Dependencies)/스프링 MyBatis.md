# 스프링 MyBatis

mybatis-spring-boot-starter

spring initializer로 설정할 때는 MyBatis Framework, MySQL Driver 2개를 설정합니다.



<br>

MyBatis는 기본적으로 Reflection을 사용합니다.

그래서, 

Reflection은 default 생성자(No arguments constructor)를 통해서 이루어지기 때문에

<br>

### MyBatis spring boot 설정방법



<br>

### INSERT할 때, 생성된 key값을 맵핑하는 방법

useGeneratedKeys="true" keyProperty="seq"

<br>

### DDD

서비스 단 로직이 있고, 

도메인의 로직은 모델이 가장 잘 안다는 것을