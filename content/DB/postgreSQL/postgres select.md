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

일단 정렬에서 [[asc,desc]] 이부분을 정확히 알아야한다.

```sql
SELECT * FROM movie 
WHERE runtime_min >= 200
ORDER BY title DESC;
```

식이 길어질 때는 이렇게 내려서 확인하는 것이 편하다.

```sql
-- 차순위 정렬
SELECT * FROM movie  
WHERE runtime_min >= 200  
ORDER BY runtime_min DESC, title DESC;
```

이런식으로 1순위 정렬, 2순위 정렬... 이렇게 넣을 수도 있다.

여러개의 정렬을 할 때에는 이렇게 컴마를 이용하면된다.


---


더 알아보기,

## 5. 몇 개 인지 세어보기

```sql
SELECT count(*) FROM movie  
WHERE release_year = 2011;
```

엑셀과 거의 비슷하다. count로 그냥 세어볼 수 있다.


## 6. 중간에 '인'이라는 글자가 들어있는 영화찾기

```sql
SELECT * FROM movie 
WHERE title LIKE '%인%';
```

LIKE는 닮았다. 이거 들어가는것과. 이런 뜻이다

앞이든 뒤든 중간이든 상관없이 찾아오겠다는 말이다

어려워도 속상해하지마  - 구글에서 "postgres 연산자"라고 검색하면 다 나오니까.

|와일드카드|의미|예시 패턴|설명|
|---|---|---|---|
|%|0개 이상의 임의의 문자|'A%'|'A'로 시작하는 모든 값|
|||'%A'|'A'로 끝나는 모든 값|
|||'%A%'|'A'를 포함하는 모든 값|
|_|정확히 1개의 임의의 문자|'A_B'|첫 글자 'A', 세 번째 글자 'B'|

그중에 ILIKE도 있는데, 니가 어떤모습이든 너무 좋아해. 라는 뜻이라서

대소문자를 구분하지않는다.


