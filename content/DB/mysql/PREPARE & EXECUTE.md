# PREPARE

SQL문의 미리 준비하는 단계를 말한다.
쿼리의 구조가 고정되고, 실행했을 때 동적으로 값을 제공할 수 있는 자리표시자(?)를 사용할 수도 있다

```mysql
PREPARE happy FROM `SELECT * FROM users WHERE id = ?`;
```

위 코드에서 happy는 sql문의 이름을 말한다.


# EXECUTE

준비된 sql문을 실행하는 단계이다. 
여기서는 자리표시자로 지정해둔 곳에 값을 실제로 할당하여 처리한다

```mysql
SET @user_id = 1;
EXECUTE happy USING @user_id;
```

여기서 만든 변수를 사용하여 값을 할당했다.
