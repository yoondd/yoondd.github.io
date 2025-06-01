C.U.D에 쓰는 저장 프로시저, 그냥 프로시저라고 부른다

구글에서 검색해라 - 개발자는 구글링을 잘해야한다

어떤 프로시저든 부분부분 나누어져있다

어차피 DB는 데이터를 관리하는애라서 계산하거나, if문, for문 같은건 좀 투박하다

SQL은 무조건 일자로 쭉쭉 실행이된다

즉, 비동기없이 무조건 "동기"로 이루어진다고 보면 된다

```sql
DECLARE
  -- 선언부: 변수, 상수 쓸 것들을 만들어준다
BEGIN --시작(여기서 끝 사이에 sql문이 이빠이 들어간다)
  -- 실행 부분: 실행 코드 작성
EXCEPTION
  -- 예외 처리부: 예외 처리 코드 작성
END; --끝
```



| **종류**              | **설명**                                                      |
| ------------------- | ----------------------------------------------------------- |
| **함수 (Functions)**  | 데이터베이스에서 자주 사용되는 작업을 수행하기 위해 사용되며 주로 ‘계산’을 수행하기 위해서 사용이 됩니다 |
| **절차 (Procedures)** | 함수와 유사하며 주로 ‘작업’을 수행하기 위해서 사용이 됩니다.                         |
| **트리거 (Triggers)**  | 데이터베이스에 대한 이벤트가 발생했을 때, 자동으로 실행되는 코드입니다.                    |


트리거는 쓸 일이 없다고 보면된다 - 이걸 그냥 프로시저로 만든다



## 프로시저의 틀


```sql
CREATE [OR REPLACE] FUNCTION function_name (arguments)
RETURNS return_datatype AS $$
DECLARE
    -- 변수 선언
BEGIN
    -- 함수 로직
END;
$$ LANGUAGE plpgsql;
```

이걸 보면서 그냥 잘 써라


## 선언부 만들기

```sql
create or replace function set_point(point_type varchar(20), trade_point double precision, member_id bigint)
returns void 
```

- 대부분 or replace를 적는다
- 개발문서 읽을 때 `[]`이게 있으면 있어도 되고 없어도 되고 이런 뜻이다
- name지을 때는 - 테이블 이름 두개를 넣을생각하지말고 행위에 대해 잘 넣어라. 
	예를 들어 포인트를 집어 넣는다 - set
	카멜은 쓰지말고 팟홀케이스를 써라 
	set_point라고 하면 행위에 대해 잘 정의해 놓은 게 된다
- 메서드라고 보면된다. 파라미터도 있잖아. 
- postgres자료형을 구글링해서 타입을 지정하라(파라미터 자리) - enum은 모른다

- returns는 반환타입을 말한다 s 가 붙는 이유는 복수일수도 있어서다
- returns에는 varchar, text도 가능하지만 table()같은것도 된다.
- 우리는 void를 사용할거라서 구글링했다 -postgres returns void
- 그렇다고 returns를 table로 써서 - 뷰테이블 대신에 쓰지는 말아라. 


여기까지가 ==선언부==를 완성한 것이다

--- 
