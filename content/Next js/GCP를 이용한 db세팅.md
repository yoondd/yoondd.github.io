클라우드 쉘을 이용해서 db 을 세팅하고 db 파일을 하나 만들어보자.

```sh
gcloud sql connect root --user=root 
```

이렇게 넣고 비밀번호 넣어서 접속해라.

```mysql
use root;
```


```mysql
create table users(
    id int auto_increment primary key,
    email varchar(255) not null unique,
    password varchar(255) not null,
    name varchar(100) not null,
    created_at timestamp default current_timestamp
);
```

```sql
insert into users (email, password, name) values ("test@test.com", "test1111", "yoon");
insert into users (email, password, name) values ("test2@test.com", "test2222", "kim");

select * from users;
```



---


## 환경변수파일만들어서 db 정보 넣기

nextjs1안에 .env파일을 만들어넣자

```js
DB_HOST = 34.41.55.78 //gcp - sql - 연결 - 공개아이피: 34.41.55.78
DB_USER = root  
DB_PASSWORD = <비번>
DB_NAME = root
```



## 전역으로 사용한  db파일 만들기

lib안에 db접속파을 뺄거다.
왜냐면 모든 컴포넌트에서 이 db를 전역적으로 사용하기 위함이다.
`lib > db.ts` 파일을 만들면된다.

```ts
import mysql from 'mysql2/promise';  
  
//nextjs에서는 mysql으로 젒속할 떄 이렇게 쓴다  
export const db = mysql.createPool({  
    host: process.env.DB_HOST,  
    user: process.env.DB_USER,  
    password: process.env.DB_PASSWORD,  
    database: process.env.DB_NAME  
})
```


---

## 환경변수에 JWT암호화 추가

jwt가 가지고 있는 토큰 자체를 암호화하고싶어.
비번만 있는 것은 아니니까. 

```ts
JWT_SECRET = 
```

요기에 암호화를 넣어줄건데, 기본값넣어주는 프로그램이 있다 (리눅스 자체 명령어)

```sh
openssl rand -base64 32 
```

이렇게 치면 무슨 암호화된 코드가 나오는데, 
그거를 저 안에 넣어주면된다.

```ts
JWT_SECRET = IBkGrPJzXYnDmwZAIZPkACi5RpbXGSTKc7heQrmZZrs=
```