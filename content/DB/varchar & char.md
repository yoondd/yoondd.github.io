
# char
- 고정길이 문자열
- char(8)으로 선언하는 경우, 2글자만 저장해도 나머지 6자리는 공백으로 채워짐
- 데이터 길이가 언제나 고정적인 주민번호, 사원번호 이런거면 적합한 선택
- 고정된 공간이다보니까 예전에는 varchar보다 검색 및 read 속도가 좀 빨랐지만, 요즘에는 컴퓨터가 좋아지면서 속도 차이가 거의없어서 varchar를 많이 쓰는 편이다
- 문자열비교시에 저장값 뒤쪽 공백도 모두 포함한다
- MySQL기준으로 최대 255글자만 저장가능하다

# varchar 
- 가변길이
- varchar(8)으로 선언했다고 볼때, 2글자를 저장하면 2바이트만 사용한다. 그러면 왜 지정하는가? - 궁금했는데 이건 ==최대길이==를 지정하는거다. 그렇게 해야만 더 긴 데이터 저장되는걸 방지할 수 있다. 우편번호를 너무 길게 입력하지 못하도록 하는 경우가 이에 해당한다. 당연히 메모리관리 차원에서도 좋고말이다.
- 데이터 길이를 확인하는 연산이 추가로 필요해서 미세하게 느리지만 아주 미세하기때문에 크게 신경안써도된다.
- 끝에 공백이있으면 다른 값으로 취급한다
- 저장공간을 절약할 수 있어서 메모리 효율이 좋다


