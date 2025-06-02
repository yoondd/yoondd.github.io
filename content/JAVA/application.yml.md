
db연결을 위해 쌤이 주신 파일

```java
spring:
  mvc:
    pathmatch:
      matching-strategy: ant_path_matcher
  datasource:
    url: jdbc:postgresql://${DB-IP}:${DB포트번호}/${DB이름}
    username: ${DB유저명}
    password: ${DB유저비밀번호}
    hikari:
      maximum-pool-size: 10
    driver-class-name: org.postgresql.Driver
  jpa:
    generate-ddl: true
    hibernate:
      ddl-auto: update
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    properties:
      hibernate:
        temp:
          use_jdbc_metadata_defaults: false
        jdbc:
          lob:
            non_contextual_creation: true
    show-sql: true
    open-in-view: false
```

