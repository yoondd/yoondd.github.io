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
	**OR REPLACE**: 만약 이미 같은 이름의 함수(또는 뷰 등)가 존재하면,  그 기존 객체를 **덮어쓴다(교체한다)** 는 뜻이다.
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

## 실행부 만들기

자바의 메서드선언처럼, 프로시저의 윗부분(대가리)을 선언했다.

그러면 이제 그다음을 해볼까?

외우지말고, 인터넷 찾아보면서해라. - 좋은건 북마크해두고~ ㅎㅎ


```sql
create or replace function set_point(point_type varchar(20), trade_point double precision, member_id bigint)
returns void as $$

$$ LANGUAGE plpgsql;
```

우리는 `{}`같은게 없잖아. 그 역할을 하는것이 바로 `as $$ $$`다.

따라서 이렇게 `as $$ ~~~~~ $$ LANGUAGE plpgsql` 하면된다

sql끝에 항상 `;`있는거 빼먹지말고!


이렇게 하다보면 꽤 헷갈리니까 권장은 대문자로 쓰는 것이 좋다



### SQL 프로시저 기본 형식

```sql
CREATE OR REPLACE function set_point(point_type VARCHAR(20), trade_point double precision, member_id bigint)
RETURNS void AS $$

$$ LANGUAGE plpgsql;
```

이렇게 선언을 하고 일을 시작해라.

프로시저 기본형태 바로 위와 같다.


좀 더 보기쉽게 해볼까?

```sql
CREATE OR REPLACE function set_point(
	point_type VARCHAR(20), 
	trade_point double precision, 
	member_id bigint
) RETURNS void AS $$
	
$$ LANGUAGE plpgsql;
```


#### (1) 변수 필요한가? 판단해보아라.

자, 지금부터 해볼 작업을 말로 먼저 설명하면.

*3개를 받아서 먼저 insert에 담아주고, 포인트 타입에 따라서 멤버 포인트를 더해줄지 빼줄지 if문으로 결정해주고 그런다.*

이 상황에서는 변수가 필요없네? `DECLARE` 는 필요없다는 소리야. 

declare를 생략하면 변수따윈 필요없다는 소리다

그래도 시작과 끝은 있어야하니까 begin / end 는 존재한다.

```sql
CREATE OR REPLACE function set_point(
	point_type VARCHAR(20), 
	trade_point double precision, 
	member_id bigint
) RETURNS void AS $$
	BEGIN
	
	END;
$$ LANGUAGE plpgsql;
```


#### (2) 기본 DML을 써볼까?

우선, insert를 써야하잖아.  (여기서 잠깐, insert는 ddl/dml/dcl중에 뭐야? [[DDL, DML, DCL]])

세개를 받아서 넣어줘야한다고.

```sql
CREATE OR REPLACE function set_point(
	p_point_type VARCHAR(20), 
	p_trade_point double precision, 
	p_member_id bigint
) RETURNS void AS $$
	BEGIN
		-- 회원포인트 테이블에 데이터 넣기
		INSERT INTO point_history (point_type, trade_date, trade_point, member_id) VALUES (p_point_type, NOW(), p_trade_point, p_member_id);
	
	END;
$$ LANGUAGE plpgsql;
```

insert를 넣어주려고하니까 위쪽에 파라미터랑 이름이 같아서 헷갈린다, 

따라서 매개변수 앞에  `_p`를 붙여주자

현재시간은 어떻게 넣어주지? - 이것도 검색해 "postgres datetime now" 이렇게 찾아.

==DML하나당 세미콜론 꼭 찍어주자==

그리고 주석처리도 꼭 해줘. sql에서 주석은 `--`을 입력해서 해야한다


#### (3) if문을 한번 써볼까?

point_type에 따라서 증가인지 감소인지 나오니까 if이나 switch가 있어야해

"postgres if"라고 검색해서 알아봐. 이렇게 검색해서 쓰는걸 한번 더 강조하겠다^ ^


```sql
CREATE OR REPLACE function set_point(
	p_point_type VARCHAR(20), 
	p_trade_point double precision, 
	p_member_id bigint
) RETURNS void AS $$
	BEGIN
		-- 회원포인트 테이블에 데이터 넣기
		INSERT INTO point_history (point_type, trade_date, trade_point, member_id) VALUES (p_point_type, NOW(), p_trade_point, p_member_id);

		-- 회원 테이블에 포인트 업데이트
		IF p_point_type = "USE" THEN
			UPDATE member SET member_current_point = member_current_point - p_trade_point 
			WHERE id = p_member_id;
		ELSIF p_point_type = "CHARGE" OR p_point_type = "WIN" THEN
			UPDATE member SET member_current_point = member_current_point + p_trade_point 
			WHERE id = p_member_id;
		end if;

	END;
$$ LANGUAGE plpgsql;
```

여기는 `a==b`이런거 없다. 똑같고 뭐고 그런거 없다.

sql은 ==함께쓰는 엑셀==이라는 사실을 잊지말자

if문을 잘 보면 우리는 `{}`이런게 없으니까 THEN같은걸로 작성한다고 보면된다.

`ELSIF`등의 이상하게 생긴문법이 있으므로 외우려고 하지말고, 어려우면 찾아서 작업하도록 해라.

나중에 익숙해 지겠지 뭐. 중요한건 로직이니까.


여기까지 작성하면, set_point() 프로시저를 불렀을 때, 데이터 넣어주고 포인트 업데이트까지 알아서 해준다.


---

여기까지 하고나서 실행버튼을 누르면된다.

자, 이걸 어디서 확인하는가. - routines라는 폴더에 가면 거기에 다 모여져 있어.

얘를 이제 호출하는걸 알아볼까?

기본적으로 가져와서 결과를 보는 것이니까, select를 써서 확인한다


```sql
--sql console
SELECT set_point("WIN", 10000, 5);
```

이렇게 실행을하면 void니까 뭔가를 리턴해내지는 않지만 실제 데이터는 정확히 입력이 된 것을 확인할 수 있다. 


시간이 좀 이상하거든? utc라서 그런것이다.

실제로는 실행해보니 잘 되는 것을 확인할 수 있다. 멋진데??


---

### 프로시저를 수정할 때에는?

가능하다. 좌측 파일리스트에서 해당내용을 우클릭한 뒤, 클립보드에 복사해다가 쓸 수있다.

