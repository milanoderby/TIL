# JPA save 메소드의 동시성 문제로 인한 Exception 발생

### Phantom Read 문제란?

Transaction Isolation Level 에 따라 여러가지 문제가 발생할 수 있습니다.

1. Dirty Read: Isolation Level 이 **Read Committed** 보다 낮을 때, 발생
2. Nonrepeatable Read: Isolation Level 이 **Repeatable Read** 보다 낮을 때, 발생
3. Phantom Read: Isolation Level 이 **Serializable** 보다 낮을 때, 발생

각각의 Transaction Isolation level 에 따른 여러가지 문제들에 대한 설명은 [다음 문서]() 를 참고하세요.

그 중 Phantom Read 문제는 Transaction을 동시에 수행하는 2개의 Thread에 대해서 다음과 같은 문제가 발생하는 현상을 말합니다.

Thread1이 Read 후, Thread2가 Add/Remove 연산을 수행하여 Thread1은 Thread2 가 Add/Remove한 결과를 인지하지 못하고, 실제와 다른 결과를 반환하는 문제을 의미합니다. 즉, 실제 데이터와 맞지 않는 문제가 발생합니다.

<br>

### JPA의 save 메소드 수행과정

JPA save(newObject) 메소드는 다음 수행과정을 거칩니다.

1. 새로운 객체 newObject의 Id 컬럼을 이용하여 DB에 해당 객체가 저장되어있는지 확인합니다. (select 쿼리 수행)
2. 기존 객체가 존재하지 않으면, insert 쿼리를 수행합니다.
3. 기존 객체가 존재하면, update 쿼리를 수행합니다.

<br>

### JPA의 save 메소드를 동시에 여러번 호출하는 경우, Exception 발생

#### 문제상황 1 - Transaction level 로 인한 Phantom Read 문제 발생가능성

Transaction Isolation Level 이 **Serializable** 보다 낮을 경우, Phantom Read 문제가 발생합니다.

우리가 사용 중인 MySQL 5.x 버전의 경우, Isolation Level 이 **Repeatable Read** 로 설정되어 있기 때문에 Phantom Read 문제가 발생할 수 있습니다. (기본 값을 그대로 사용하고 있습니다.)

DB 의 transaction isolation level 설정은 다음 명령어로 확인가능합니다.

```sql
show variables like '%isolation%'
```

![image-20220504194117399](./images/phantom-read-2)

<br>

위에서 확인했던 것처럼 JPA save 연산은 수행 시, select 쿼리를 먼저 수행하여 해당 객체가 DB에 존재하는 객체인지 확인합니다.

예를 들어 3개의 Thread에서 동시에 JPA save 연산을 호출하는 경우, 각각의 Thread에서 수행되는 select 구문은 모두 DB에 객체가 저장되지 않았으니 null 값을 반환할 것입니다. 그리고, 3개의 Thread는 insert를 수행하는 쿼리를 수행할 것입니다. 즉, 각각의 Thread가 새로운 레코드가 추가되는 것을 인지하지 못하는 **Phantom Read 문제**가 발생하여 레코드 추가 행위를 수행하려고 하는 것입니다.

<br>

#### 문제상황 2 - 동시에 수행하는 insert 문의 Primary Key가 중복되는 상황

동일한 insert 문 3개가 동시다발적으로 들어와서 Exception이 발생하는 경우는 Primary Key가 겹치는 상황이기 때문입니다.

이 상황에서 DB에 수행되는 3개의 insert 쿼리 중 처음에 수행되는 쿼리만 정상동작하며, 나중에 수행되는 2개의 쿼리는 Duplicate Key (키 중복) Exception을 발생시킬 것입니다.

![image-20220504185400572](./images/phantom-read-1)

<br>

그 이후에 수행되는 JPA save 연산은 select 쿼리를 통해 DB에 저장하려는 객체가 존재한다는 것을 인지하게 되니, update 쿼리를 수행할 것입니다.

<br>

### 해결방안

#### 1. Isolation Level 을 Serializable 로 변경

```java
@Transactional(isolation = Isolation.SERIALIZABLE)
fun saveObject(newObj : newObject) : newObject {
	return objRepository.save(newObj)
}
```



<br>

#### 2. 

<br>