SQL에서 **CHECK**는 테이블의 컬럼에 저장될 수 있는 값에 대해 **조건(제약)**을 거는 기능을 말한다. 
즉, 특정 컬럼에 들어갈 값이 반드시 조건을 만족해야만 저장될 수 있도록 제한한다.

---

## 예시 설명

```sql
gender CHAR(1) CHECK (gender IN ('M', 'F'))
```

- **gender** 컬럼에는 반드시 'M'(수컷) 또는 'F'(암컷)만 저장할 수 있다
- 만약 'X', 'A', 또는 아무 문자나 넣으려고 하면 데이터베이스가 오류를 내고 저장하지 않는다

---

## CHECK의 역할

- **데이터 무결성(Data Integrity)**을 보장한다
- 잘못된 값이 테이블에 저장되는 것을 사전에 차단한다

---

## 다른 예시

```sql
age INT CHECK (age >= 0)
```

- age는 0 이상만 입력 가능



```sql
score INT CHECK (score BETWEEN 0 AND 100)
```

- score는 0~100 사이만 입력 가능


---

## 요약

**CHECK**는

> "이 컬럼에 들어가는 값은 반드시 이런 조건을 만족해야 해" 
> 라는 규칙을 테이블에 부여하는 SQL 제약조건을 말하는 것이다



토끼를 관리하기 위해 만든 테이블에서

```sql
CREATE TABLE rabbits (  
    id BIGSERIAL PRIMARY KEY,  
    rabbit_name VARCHAR(15) NOT NULL,  
    gender CHAR(1) CHECK (gender IN ('M', 'F')),  
    birth_date DATE NOT NULL,  
    is_alive BOOLEAN NOT NULL  
)
```

이렇게 사용했다.