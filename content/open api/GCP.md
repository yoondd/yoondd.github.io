1. 콘솔에서 사용해보기
	http://cloud.google.com/free?utm_source=google&utm_medium=cpc&utm_campaign=japac-KR-all-ko-dr-BKWS-all-core-athena-EXA-dr-1710102&utm_content=text-ad-none-none-DEV_c-CRE_668690472449-ADGP_Hybrid+%7C+BKWS+-+EXA+%7C+Txt+-GCP-General-core+brand-main-KWID_43700077514871058-kwd-87853815&userloc_9214122-network_g&utm_term=KW_gcp&gad_source=1&gclid=Cj0KCQjw2N2_BhCAARIsAK4pEkW4rf_1TBNHbh6EFZlosOwhEnA-s2KZa2HsUU1nwH_EqyddiBSyxUwaAiLQEALw_wcB&gclsrc=aw.ds&authuser=1

2. 클라우드 쉘을 사용할 수 있어야한다. (우측 상단)

3. 왼쪽에 햄버거 바에서 클릭클릭하면서 쓰는 방법으로도 쓸 수 있다

4. gcp에서 sql공간을 하나 얻어서 테이블을 만들 것이다. 
	환경이 다를 수있으니까 gcp스토리지에 우리가 도커파일을 만들어서 올리고, 그러면 api가 하나 떨어진다.
	그걸 우리 서버파일에다가 올린다. 이걸 진행하면 gcp와 우리의 컴퓨터가 서로 교류할 수 있게된다.

5. sql 인스턴스 만들기  
- Enterprise 우측을 누른다.  pro말고.
- 샌드박스 선택(제일 저려한 것)
- mysql 8.0
- root/ 비번(복잡성 선택 후 복잡하게)
- 리전: 아이오와 --- 실제 서버가 있는 위치/ 물가 싼나라가 최고다
- 머신 구성 CPU 1개
- 데이터 캐시는 사용해제로 그대로 유지하기
- 저장용량은 HDD로 변경하고 10GB 유지
- 연결은 비공개와 공개 둘다 해두기
- 네트워크추가하기. any(이름 아무거나해도 상관없음) / 누구나 접근가능하도록 0.0.0.0/0으로 일단설정
- 데이터보호는 ==자동백없이 되지않게== 설정하기!

4. 기본인스턴스 사이드바 '연결'에서 나오는 ==공개 IP주소==가 나의 sql로 들어올 주소라고 보면 된다.

5. 사이드바에서 사용자를 확인해보고, 데이터베이스로 들어가서 데이터베이스를 만든다
	우리는 root라는 데이터베이스를 만들었다

6. 사용자 메뉴로 가서 root의 비밀번호를 변경했다
	*내가 사용하는 비밀번호 중에 최고 난이도의 비밀번호로...🤨*



