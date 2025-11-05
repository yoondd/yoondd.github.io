
저장된 데이터를 드디어 db에서 확인해볼까?

1. db에서 확인하려면 먼저 접근설정을 해야해

/Users/yunhyegyeong/Developer/firstproject/src/main/resources/application.properties

```java
#h2 DB, 웹 콘솔 접근 허용
spring.h2.console.enavled=true;
```

1. [localhost:8080/h2-console/에서](http://localhost:8080/h2-console/%EC%97%90%EC%84%9C) 확인할 수 있어

![](https://i.imgur.com/vIPlZ7Q.png)


---

데이터를 더미로 여러개 만드는 방법

/Users/yunhyegyeong/Developer/firstproject/src/main/resources/data.sql