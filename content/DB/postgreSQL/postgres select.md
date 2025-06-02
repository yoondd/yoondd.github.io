데이터를 넣었으면 이제 꺼내서 봐야하잖아.

지금까지 공부한 내용을 참고해야함 
- [[postgres create]]
- [[postgres insert]]


## SQL 문법 순서
- SELECT 
- FROM 어디에서 꺼낼 건지. 테이블이름이 나오겠지.
- WHERE 조건
- GROUP BY 이거는 JOIN이랑 같이 가는건데 알아볼 것이다
- HAVING 얘는 GROUP BY랑 같이 가는애고
- ORDER BY 정렬하는애다


SQL실행순서에 대한 것은 나중에 천천히 알아보자.

어떻게 돌아가는지 알아야하거든.


---


## 1. 전체 가져와서 확인하기

그냥 모든걸 다 가져와서 보고싶다면.

```sql
SELECT * FROM movie;
```


## 2. 전체 리스트에서 필요한 컬럼만 보기

```sql
SELECT title, id FROM movie;
```

순서랑은 아무 상관이없이 그대로 내 맘대로 가져오면된다.

이래서 순서가 없다는 말이다.


## 3. 조건에 맞는 것만 가져오기

```sql
SELECT * FROM movie WHERE runtime_min >= 200;
```

이렇게 런타임이 긴 것만 가져오면된다.


## 4. 정렬해서 가져오기

