수업영상: 20250204_1강 DataGrip 시작


postgres에서 create문을 이용해 예제를 구현해보자.

영화를 담는 테이블을 한번 만들어볼까?

```sql
CREATE TABLE movie(){
	id BIGSERIAL PRIMARY KEY,
	title VARCHAR(50) NOT NULL,
	release_year INTEGER NOT NULL,
	director VARCHAR(50) NOT NULL,
	runtime_min INTEGER NOT NULL
}
```

### BIGSERIAL에 대해 한번 알아본다면,
[[BIGSERIAL]]
auto increament가 없구나. postgres에는!


이렇게 완성할 수 있다. 

실행하고 새로고침하면 테이블이 생성된 것을 볼 수 있다.

여기서 중요한건 `CREATE TABLE`을 이용해서 간단하게 테이블을 생성할수 있다는 것이다.


