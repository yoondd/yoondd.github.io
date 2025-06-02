이 기능은 엑셀에서 피벗테이블과 비슷하다

그룹으로 묶어서 통계내거나 할 때 사용한다

```sql
SELECT
	lh.create_at,
	COUNT(lh.rabbit_id) AS count_rabbit
FROM link_history lh
GROUP BY lh.create_at
ORDER BY lh_create_at;
```

피벗테이블이기때문에 저 위에는 합산이 가능한영역만 들어갈 수 있다

갑자기 `lh.rabbit_id` 같은게 들어갈 수 없다는 말이다


GROUP BY가 묶어준다는 것은 쉽지만, 이걸 왜 쓰는지를 알아야한다

이건 통계, 집계를 위해서 사용한다는 것이 중요하다 - 마치 피벗테이블처럼 말이다




성별, 출생년도, 생사여부 그룹에 따른 토끼수 구해봐바. 