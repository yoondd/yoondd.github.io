

![](https://i.imgur.com/7PBOvgT.png)



자바에서는 JPA를 가지고 SQL을 제어하고,

노드는 시퀄라이저를 이용하고,

파이썬은 장고를 이용하고…

사실 자바에서도 뒤에가 h2가되던 oracle이되던 mysql이되던 상관이없어.

그저 우리는 JPA만 만지면되자나. 

oracle전용,  h2전용..이런걸 짜는것보다 훨씬 효율적이지. 

생각해봐. 실무에서 db단이 달라지면 어쩔껀데. 

시퀄라이져, JPA같은것들을 ORM이라고하거든. 이거 좋긴한데, 어찌되었던 번역되어서 거쳐서 나가니까 불편하기도해.

- ORM모델의 장점: 편하다. (SQL문법 몰라도되니까)
- ORM모델의 단점: 느리다. (번역을 해야되자나…) 또, ORM이 바뀌면 또다른 명령어를 외워야하자나

---

데이터 crud의  sql쿼리문

![](https://i.imgur.com/azTaiHl.png)

/Users/yunhyegyeong/Developer/firstproject/src/main/resources/application.properties

# 로그에서 sql문 확인하는 설정

```java
#JPA 로깅 설정
# 디버그 레벨로 쿼리 출력
logging.level.org.hibernate.SQL = DEBUG
# 이쁘게 보여주기
spring.jpa.properties.hibernate.format_sql=true
# 파라미터 보여주기
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.orm.jdbc.bind=TRACE
```

# db접속할때마다 바꿔야하는 값 고정하기

```java
#DB URL 고정 설정
# 유니크 url 생성 x
spring.datasource.generate-unique-name=false
# 고정 url설정
spring.datasource.url=jdbc:h2:mem:testdb
```

# db가 아이디값을 자동으로 생성할 수 있게.

```java
 //private Long id; //구분하기위한 대표값(기본키)
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // db가 id를 자동생성한다 
    private Long id;
```

entity에 전략을 설정한다. 이건 진짜 필수다.