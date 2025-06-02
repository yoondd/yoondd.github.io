## REFERENCES란? (SQL 외래키 제약조건 설명)

**REFERENCES**는 SQL에서 **외래키(Foreign Key) 제약조건**을 지정할 때 사용하는 키워드입니다.  
외래키는 한 테이블의 컬럼이 다른 테이블의 기본키(Primary Key)와 연결되어 있음을 의미합니다.

---

## 1. 역할

- **데이터의 무결성 보장:**  
    한 테이블의 값이 반드시 다른 테이블에 존재하는 값이어야 함을 강제합니다.
    
- **테이블 간 관계 설정:**  
    두 테이블을 연결(참조)하여, 데이터베이스의 ==관계형 구조를 구현==합니다.
    

---

## 2. 예시 설명

```sql
CREATE TABLE link_history (
    id BIGSERIAL PRIMARY KEY,
    rabbit_id INTEGER NOT NULL REFERENCES rabbits(id),
    customer_id INTEGER NOT NULL REFERENCES customer(id),
    create_at DATE NOT NULL
);

```

- **rabbit_id INTEGER NOT NULL REFERENCES rabbits(id)**  
    → link_history 테이블의 rabbit_id 값은 반드시 rabbits 테이블의 id 컬럼에 존재하는 값이어야 합니다.
    
- **customer_id INTEGER NOT NULL REFERENCES customer(id)**  
    → link_history 테이블의 customer_id 값은 반드시 customer 테이블의 id 컬럼에 존재하는 값이어야 합니다.
    

---

## 3. 예시 상황

- rabbits 테이블에 id가 5인 토끼가 없으면, link_history에 rabbit_id=5로 데이터 입력 시 오류 발생.
    
- customer 테이블에 id가 10인 고객이 없으면, link_history에 customer_id=10으로 입력 시 오류 발생.
    

---

## 4. 장점

- **잘못된 참조 방지:**  
    존재하지 않는 토끼나 고객을 링크 기록에 남기는 실수를 막을 수 있음.
    
- **데이터 일관성 유지:**  
    테이블 간의 논리적 연결을 보장.
    

---

## 요약

**REFERENCES**는

> 이 컬럼의 값은 반드시 다른 테이블의 특정 컬럼(주로 Primary Key)에 존재해야 해!  
> 라는 규칙을 부여하는 외래키 제약조건입니다.

이렇게 하면 데이터베이스 내에서 테이블 간의 관계와 데이터의 정확성을 자동으로 관리할 수 있습니다!
