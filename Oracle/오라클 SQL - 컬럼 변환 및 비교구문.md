# 오라클 SQL - 컬럼 변환 및 비교구문

## date - varchar2 타입 간의 변환

### to_char() 함수

#### 문법

to_char(date 값, '변환할 문자열형태')

<br>

#### 예시

```sql
select to_char(sysdate, 'YYYY-MM-DD HH24:MI:SS')
from dual;

// 출력결과
2022-02-08 16:54:05
```

<br>

### to_date() 함수

#### 문법

to_date(문자열, '파싱될 문자열 형태')

<br>

#### 예시

```sql
select to_date('2022-02-08 16:54:05', 'YYYY-MM-DD HH24:MI:SS')
from dual;

// 츨력결과
2022-02-08 16:54:05
```

<br>

## number - varchar2 타입 간의 변환

### to_char() 함수

#### 문법

to_char(숫자 값)

<br>

#### 예시

```sql
select to_char(12)
from dual

// 출력결과
12
```

<br>

### to_number() 함수

#### 문법

to_date('숫자로만 이루어진 문자열')

<br>

#### 예시

```sql
select to_number('12')
from dual

// 츨력결과
12
```

<br>

#### ORA-01722: 수치가 부적합합니다 오류

숫자로만 이루어지지 않은 문자열을 `to_number()` 함수로 컬럼 변환 중 발생하는 오류입니다.

```sql
select to_number('1,2')
from dual

// 아래 오류 발생
ORA-01722: 수치가 부적합합니다
```

<br>

## 주의사항

### join연산에서 컬럼 변환 사용 시, 주의사항

`테이블A` join `테이블B` 라고 했을 때, `테이블A`의 컬럼에서 `테이블B` 컬럼으로 변환할 수 없는 데이터가 있을 경우, 전체적으로 오류를 발생합니다.

그래서, 모든 데이터 값에 대해서 정확하게 유효성이 확인될 때에만 변환연산을 사용하는 것을 권장합니다.
