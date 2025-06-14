토끼예제에 이어서 하는 공부 [[토끼 입양시키기]]

우리에게는 테이블이 3개가 있다.

토끼, 입양내역, 입양자.

따로따로 용도에 맞는 정보가 나누어져있다는 말이다.

근데 내가 보고싶은건, 입양완료된 토끼와 - 해당 토끼의 입양자다.

우리는 이걸로는 알수가 없는데, 어떡하지? - 이때 바로 JOIN이 필요하다

결국 join은 다른 테이블을 가져오는거다.


모든 join은 주동자가 있다.

그걸 우리는 ==메인테이블==이라고 부른다

> 입양 완료된 토끼와 해당 토끼의 입양자를 조회하고싶다

자 여기서 주동자는 누구일까? 바로 토끼가 아닌가.  토끼에 대한 정보를 보고싶은거잖아

즉, 메인테이블을 "토끼"테이블로 잡아야한다

따라서 엑셀을 이용해서  간단히 계획을 세워보자

## 1. 토끼테이블 그대로 긁어오기
![](https://i.imgur.com/yrqmIFh.png)


## 2. 입양자 테이블 그대로 긁어오기
![](https://i.imgur.com/Wa4uPPs.png)


## 3. 중간다리 역할의 아이를 땡겨서 긁어오자
![](https://i.imgur.com/vepOy0P.png)



## 4. 먼저 주동자와 중간다리를 엮어주자
쉽게말해, 중간다리 아이디는 일단 필요없고, 토끼테이블 id와 중간다리 rabbit_id가 연관이 있는 것이 아닌가. 이들을 합쳐주면된다
![](https://i.imgur.com/qWHT35J.png)



## 5. 중간다리와 나머지를 엮어주자
그러면 모두 쭉 완성이 되네.
![](https://i.imgur.com/xBuNGXZ.png)


---

자, 위의 계획을 이번에는 코드로 나열해보자

## 1. 토끼테이블과 중간다리 붙이기

```sql
SELECT  
    *  
FROM rabbits r  
JOIN link_history lh ON r.id = lh.rabbit_id;
```

## 2. 입양자 테이블과 중간다리를 붙이기

```sql
SELECT  
    *  
FROM rabbits r  
JOIN link_history lh ON r.id = lh.rabbit_id
JOIN customer c ON lh.customer_id = c.id;
```

## 3. 내가 필요한 정보만 빼오기

```sql
SELECT  
    r.rabbit_name, 
    r.gender,
    r.birth_date,
    r.is_alive,
    lh.create_at,
    c.name,
    c.phone_number
FROM rabbits r  
JOIN link_history lh ON r.id = lh.rabbit_id
JOIN customer c ON lh.customer_id = c.id;
```

## 4. 당연히 조건도 걸 수 있다

```sql
SELECT  
    r.rabbit_name,  
    r.gender,  
    r.birth_date,  
    r.is_alive,  
    lh.create_at,  
    c.name,  
    c.phone_number  
FROM rabbits r  
JOIN link_history lh ON r.id = lh.rabbit_id  
JOIN customer c ON lh.customer_id = c.id  
WHERE r.is_alive = true;
```


---

실제 업무할때 이 조인만을 많이 쓴다. 

## 이너조인

join은 일단 양쪽에 있어야만 가능하다

합석할때 양쪽에 남자, 여자가 있어야 가능한것과도 같다.

그래서 조인의 기본은 이너조인이다 ( 따로 언급안하면 이너조인 )


## left조인

`from A left join B` 하면 A는 무조건 다 있어야한다

주동자는 다 보이고, left로 잡힌애는 있는것만 보이는것이다 없으면 null로 다 채우고 말이야.

---




## 나혼자 해보는 예제

입양되지않고 살아있는 토끼만 보기.

```sql
SELECT  
    *  
FROM rabbits r  
WHERE is_alive = true  
AND id NOT IN (SELECT rabbit_id FROM link_history);
```

뭐지 왜케어려워 ㅋㅋㅋㅋㅋ

엄밀히 말하면 얘는 join은 아니네. "서브 쿼리"라고 할 수 있다


> 결국 토끼의 아이디가 link_history에 없으면된다

https://caramel-pine-008.notion.site/SQL-JOIN-191c7daa290d80c08c9bc6810b6e44f8