# Isolation level 및 이상현상

# **트랜잭션(Transaction)이란?**

**▶ 정의**

데이터베이스에서 하나의 논리적인 작업 단위. 여러 개의 SQL 문을 묶어 하나의 작업처럼 처리함.

**▶ 트랜잭션의 4가지 성질: ACID**

| **속성** | **설명** |
| --- | --- |
| Atomicity (원자성) | 트랜잭션 내 작업이 전부 수행되거나, 전혀 수행되지 않아야 함 |
| Consistency (일관성) | 트랜잭션 수행 후에도 DB의 데이터는 일관성 있는 상태를 유지 |
| Isolation (격리성) | 트랜잭션끼리 서로의 작업에 영향을 주지 않아야 함 |
| Durability (지속성) | 트랜잭션이 커밋되면, 그 결과는 시스템 오류가 발생해도 유지 |

---

# **Isolation Level(격리 수준)이란?**

동시에 여러 트랜잭션이 수행될 때 트랜잭션 간 간섭을 얼마나 허용할 것인지 결정하는 기준

→ 격리 수준이 높을수록 데이터 일관성은 보장되지만, 성능은 낮아질 수 있음.

---

# **트랜잭션 격리 수준 4단계**

1. **Read Uncommitted**

   SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리지 않는 계층

    - 트랜잭션에 처리중이거나, 아직 Commit되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용함

   → 데이터베이스의 일관성을 유지하는 것이 불가

2. **Read Committed**

   SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리는 계층

    - 트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 됨
    - Commit이 이루어진 트랜잭션만 조회 가능

   →대부분의 SQL 서버가 Default로 사용

3. **Repeatable Read**

   트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 계층

    - 트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 동일함을 보장함
    - 다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 불가능

   →MySQL에서 Default로 사용

4. **Serializable**

   트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 계층

    - 완벽한 읽기 일관성 모드를 제공함
    - 다른 사용자는 트랜잭션 영역에 해당되는 데이터에 대한 수정 및 입력 불가능

| **격리 수준** | **Dirty Read** | **Non-repeatable Read** | **Phantom Read** | **설명** |
| --- | --- | --- | --- | --- |
| Read Uncommitted | 발생함 | 발생함 | 발생함 | 가장 낮은 수준. 커밋되지 않은 데이터도 읽음 |
| Read Committed | 방지함 | 발생함 | 발생함 | 커밋된 데이터만 읽을 수 있음 |
| Repeatable Read | 방지함 | 방지함 | 발생함 | 동일 조건 조회 시 결과 동일. 단, 새로운 행 삽입은 보장하지 않음 |
| Serializable | 방지함 | 방지함 | 방지함 | 완벽한 격리. 동시성 최소화 (성능 저하) |

**격리 수준이 높아질수록 잠금(lock)이 많아짐**

- 높은 격리 수준일수록 다른 트랜잭션이 접근할 수 없도록 더 많은 데이터에 잠금(Lock) 발생
- 동시에 실행될 수 있는 트랜잭션의 수가 줄어들어 동시성 저하

**낮은 격리 수준 (Read Committed):**

- 트랜잭션 A가 상품 재고를 조회하는 중
- 동시에 트랜잭션 B가 그 상품의 재고를 변경할 수 있음
- 동시성은 높음 → 정합성 문제(Non-repeatable Read) 발생 가능

**높은 격리 수준 (Serializable):**

- 트랜잭션 A가 상품 재고를 조회
- 트랜잭션 B는 재고에 접근 못하고 대기
- 정합성은 완벽 → 동시 실행 수가 줄어들어 성능 저하

---

# **이상 현상(Anomalies)**

**Dirty Read**

- 트랜잭션 A가 아직 커밋하지 않은 데이터를 트랜잭션 B가 읽음
- A가 롤백하면 B는 잘못된 값을 본 셈

```jsx
- 트랜잭션 A

BEGIN;

UPDATE users SET balance = 500 WHERE id = 1;

- - 커밋 전
- - 트랜잭션 B

SELECT balance FROM users WHERE id = 1; -- 500 읽음 → Dirty Read
```

**Non-repeatable Read**

- 한 트랜잭션 안에서 같은 SELECT를 두 번 실행했는데 결과가 달라짐
- 다른 트랜잭션이 사이에 데이터를 수정했기 때문

```jsx
- 트랜잭션 A

BEGIN;

SELECT salary FROM employees WHERE id = 1; -- 3000

- - 트랜잭션 B

UPDATE employees SET salary = 4000 WHERE id = 1;

COMMIT;

- - 트랜잭션 A

SELECT salary FROM employees WHERE id = 1; -- 4000 → Non-repeatable Read
```

**Phantom Read**

- 동일 조건의 SELECT 쿼리를 실행했을 때, 결과 행의 수가 달라지는 현상
- 다른 트랜잭션에서 INSERT 또는 DELETE를 했기 때문

```jsx
- 트랜잭션 A

BEGIN;

SELECT * FROM orders WHERE amount > 1000;

- - 트랜잭션 B

INSERT INTO orders (id, amount) VALUES (10, 2000);

COMMIT;

- - 트랜잭션 A

SELECT * FROM orders WHERE amount > 1000; -- 결과 달라짐 → Phantom Read
```

---

# **사용 예시**

| **시스템 유형** | **추천 격리 수준** | **이유** |
| --- | --- | --- |
| 단순 조회 시스템 (게시판 등) | Read Committed | Dirty Read만 방지하면 충분. 성능 우선 |
| 은행, 금융 거래 | Serializable | 데이터 무결성 최우선 |
| 일반 업무 시스템 | Repeatable Read | 조회 신뢰성 보장, 성능도 적절 |
| 대량 처리 시스템 | Read Committed | 성능 중요시, 다소의 데이터 변경 감수 가능 |

---

# 퀴즈

**가장 안전한 격리 수준과 그 단점은?**

---

# 출처

https://f-lab.kr/insight/database-transaction-and-isolation-level?gad_source=1&gbraid=0AAAAACGgUFfufLAuiHo12uXsj7F6Yjctv&gclid=Cj0KCQjw2ZfABhDBARIsAHFTxGxDTDk0iHsJpKaClF0qSjDh_vOzttQeQIBaSLcFS_V7EHZSyyn40bgaAhH5EALw_wcB