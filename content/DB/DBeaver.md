- 나는 공식웹사이트를 이용해서 다운로드 받았다.

1. mysql선택하고 공개ip를 서버호스트로 사용
	serverhost : <나의 공개 ip>
	database: root
	username: root
	password: <나의패스워드>

2. test connection해보기

3. sql편집기 - 새창을 하나 띄우면 이제 여기에 명령어를 넣을 수 있다

4. 테이블 하나 만들기 -  ctrl+enter
```sql
CREATE TABLE guest (
	id INT AUTO_INCREMENT PRIMARY KEY,
	name VARCHAR(100) NOT NULL,
	message TEXT NOT NULL,
	created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

5. 자료 넣고 확인해보기. 어렵지 않다. 



GCP로 mysql을 받았는데 꼭 dbeaver가 필요할까? 
아니야! 이거 없어도 쉘에서도 충분히 쓸 수 있어~ 
그 관련된 내용은 여기서 확인할 수 있지.


[[Cloud shell에서 쿼리문 작성하기]]