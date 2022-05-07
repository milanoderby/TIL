# 동시성 문제와 Spring Transaction isolation level

이 글에서는 동시성 문제로 발생할 수 있는 문제의 종류와 이를 해결하기 위해 Spring 에서는 어떤 Isolation level을 사용해야하는지 알아봅니다.

<br>

### 트랜잭션의 동시성 문제

#### Dirty Read

| T1       | Field Value      | T2         | T2 Value |
| -------- | ---------------- | ---------- | -------- |
|          | 1 (기존 필드 값) |            |          |
| Update   | 2                |            |          |
|          | 2                | Read       | 2        |
| Rollback | 1                |            | 2        |
|          | 1                | Dirty Read | 2        |

위 표를 설명하자면, 아래와 같습니다.

1. T1이 Update (Commit 되지 않은 상태)
2. T2가 Read (Commit 되지 않은 값을 읽음)
3. T1이 Rollback
4. T2가 들고 있는 값이 쓰레기 값이 됨. (Dirty Read 발생!)

이를 Dirty Read 라고 합니다.

<br>

이를 해결하기 위한 트랜잭션 격리 수준으로 **READ_COMMITTED** 가 있습니다.

이는 한 트랜잭션이 쓰기 작업을 수행하는 동안 다른 트랜잭션은 해당 레코드의 값 읽을 수 없게하여 쓰레기 값을 읽지 않도록 보장합니다.

<br>

#### Nonrepeatable Read

| T1     | Field Value      | T2                        | T2 Value |
| ------ | ---------------- | ------------------------- | -------- |
|        | 1 (기존 필드 값) |                           |          |
|        | 1                | Read                      | 1        |
| Update | 2                |                           | 1        |
|        | 2                | Read (Nonrepeatable Read) | 2        |

위 표를 설명하자면, 아래와 같습니다.

1. T2가 Read
2. T1이 Update
3. T2가 Read (Nonrepeatable Read)
4. T2가 들고 있는 값이 이전에 읽은 값과 다르게 됨. (어떤 값을 써야될지 모르게 됨)

이를 Nonrepeatable Read 라고 합니다.

<br>

이를 해결하기 위한 트랜잭션 격리 수준으로 **REPEATABLE_READ** 가 있습니다.

이는 한 트랜잭션이 작업을 수행하는 동안 다른 트랜잭션은 해당 레코드를 수정할 수 없게하여 트랜잭션이 필드를 여러번 읽어도 동일한 값을 읽도록 보장합니다.

<br>

#### Phantom Read

| T1         | Row Count        | T2                       | T2 Row Count |
| ---------- | ---------------- | ------------------------ | ------------ |
|            | 1 (기존 행의 수) |                          |              |
|            | 1                | Read Rows                | 1            |
| Insert Row | 2                |                          | 1            |
|            | 2                | Read Rows (Phantom Read) | 2            |

위 표를 설명하자면, 아래와 같습니다.

1. T2가 Read Rows
2. T1이 Insert Row
3. T2가 Read Rows (Phantom Read)
4. T2은 T1에서 삽입한 행이 보입니다.

이를 Phantom Read 라고 합니다.

<br>

이를 해결하기 위한 트랜잭션 격리 수준으로 **SERIALIZABLE** 이 있습니다.

이를 한 트랜잭션이 작업을 수행하는 동안 다른 트랜잭션은 해당 테이블에 추가/삭제를 할 수 없게하여 트랜잭션이 로우를 여러번 읽어도 동일한 행들만 읽어오도록 합니다.
