[[뷰테이블만들기]]

위 공부에서 GROUP BY를 썼어.

왜 썼는지 한번 더 정리하기위해서 GROUP BY를 한번 더 공부한다

[[GROUP BY]] 

여기서 공부를 하기는 했지만 부족했거든.


```sql
CREATE OR REPLACE VIEW member_view AS 
SELECT 
	m.id as member_id,
	m.username,
	m.nickname,
	m.real_name,
	.
	.
	.
	m.last_login_time,
	m.member_current_point,
	coalesce(sum(ph.trade_point), 0) total_point_trade --1
FROM member m
LEFT JOIN point_history ph ON m.id = ph.member_id 
GROUP BY  --2
	m.id,
	m.username,
	m.nickname,
	m.real_name,
	.
	.
	.
	m.last_login_time,
	m.member_current_point;

```

자, 여기서 GROUP BY를 왜 썼을까.


## 1. 집계 함수(aggregate function) 사용 때문

쿼리에서 아래 부분을 보면:

```sql
coalesce(sum(ph.trade_point), 0) total_point_trade
```

- 여기서 **SUM(ph.trade_point)** 는 집계 함수(aggregate function)다.
- 집계 함수는 여러 행(row)을 하나의 값으로 합치는 역할을 한다.
- 예를 들어, 한 회원이 여러 번 포인트를 거래했다면, 그 회원의 모든 거래 포인트를 합산해야 한다는 말이다

## 2. 회원별로 합계를 구하기 위해

- **LEFT JOIN**을 사용해서 `member`와 `point_history`를 연결했다.
- 한 사람이 여러 번 포인트 거래를 했으면, 그만큼 데이터가 여러 줄로 늘어난다.
- 그런데 우리는 **회원별로 포인트 거래 합계**를 구하고 싶은데 어떡하냐고. 
- 이때 **회원별로 묶어서(sum)** 계산하려면 `GROUP BY`가 필요한 것이지!

## 3. SELECT 절에 집계 함수와 일반 컬럼이 같이 있을 때

- `SELECT`에 집계 함수(SUM 등)와 일반 컬럼(예: m.id, m.username 등)이 같이 있으면,
- **집계 함수가 아닌 컬럼**들은 반드시 `GROUP BY`에 포함해야 한다  (그것이 바로 SQL 표준 규칙이니라)