
## **DDL (Data Definition Language) – 데이터 정의어**

- **정의**: 데이터베이스의 구조(스키마, 테이블, 뷰, 인덱스 등)를 생성, 변경, 삭제하는 언어

- **주요 명령어**:
    - CREATE: 데이터베이스, 테이블, 뷰, 인덱스 등 생성
    - ALTER: 기존 객체의 구조 변경
    - DROP: 객체 삭제
    - TRUNCATE: 테이블 데이터 전체 삭제(구조는 유지)
    - RENAME: 객체 이름 변경
        
- **특징**:
    - 데이터베이스의 전체 골격을 결정
    - 실행 즉시 적용, 트랜잭션 롤백 불가(일부 DBMS에서 예외)
    - 주로 데이터베이스 관리자(DBA)나 설계자가 사용

---

## **DML (Data Manipulation Language) – 데이터 조작어**

- **정의**: 데이터베이스에 저장된 데이터를 조회, 삽입, 수정, 삭제하는 언어

- **주요 명령어**:
    - SELECT: 데이터 조회
    - INSERT: 데이터 삽입
    - UPDATE: 데이터 수정
    - DELETE: 데이터 삭제
        
- **특징**:
    - 테이블 내부의 데이터(행, 레코드)를 조작
    - 트랜잭션 지원(ROLLBACK, COMMIT 가능)
    - 데이터베이스 사용자와 DBMS 간의 인터페이스 역할

---

## **DCL (Data Control Language) – 데이터 제어어**

- **정의**: 데이터베이스에 대한 접근 권한 및 보안, 무결성, 회복, 병행 제어 등을 담당하는 언어
    
- **주요 명령어**:
    - GRANT: 사용자에게 권한 부여
    - REVOKE: 사용자 권한 회수
    - (일부 분류에서는 COMMIT, ROLLBACK, SAVEPOINT 등 트랜잭션 제어 명령도 포함)

- **특징**
    - 데이터베이스 객체에 대한 접근 권한을 관리
    - 데이터의 보안, 무결성, 회복, 병행 제어 등 관리 목적
    - 주로 데이터베이스 관리자(DBA)가 사용

---

## **요약 비교표**

| 구분    | DDL (정의어)                             | DML (조작어)                      | DCL (제어어)                        |
| ----- | ------------------------------------- | ------------------------------ | -------------------------------- |
| 역할    | 구조 생성/변경/삭제                           | 데이터 조회/삽입/수정/삭제                | 권한 부여/회수, 보안, 무결성 관리             |
| 주요명령어 | CREATE, ALTER, DROP, TRUNCATE, RENAME | SELECT, INSERT, UPDATE, DELETE | GRANT, REVOKE (COMMIT, ROLLBACK) |
| 대상    | 테이블, 뷰, 인덱스 등 구조                      | 테이블의 데이터(행, 레코드)               | 사용자, 권한, 트랜잭션                    |
| 특징    | 구조 변경, 롤백 불가                          | 데이터 조작, 트랜잭션 지원                | 접근 제어, 보안, 무결성, 회복               |

---

## **예시**

## DDL

```sql
CREATE TABLE Users (
  id INT PRIMARY KEY,
  name VARCHAR(100)
);
ALTER TABLE Users ADD COLUMN email VARCHAR(255);
DROP TABLE Users;
```


## DML

```sql
INSERT INTO Users (id, name) VALUES (1, '홍길동');
SELECT * FROM Users;
UPDATE Users SET name = '이몽룡' WHERE id = 1;
DELETE FROM Users WHERE id = 1;
```


## DCL

```sql
GRANT SELECT ON Users TO user1;
REVOKE SELECT ON Users FROM user1;
```

---

**정리**:
- DDL: 데이터베이스의 "틀"을 만들고 바꾸는 언어
- DML: 그 틀 안의 "데이터"를 넣고 바꾸는 언어
- DCL: 데이터베이스와 객체에 대한 "접근 권한"을 관리하는 언어  