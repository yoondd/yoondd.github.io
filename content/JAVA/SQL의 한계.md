
> DateJigak값이 ~부터
> DateJigak값이 ~까지
> DateJidak값이 ~부터 ~까지
> 이를 하나의 메서드로 처리할 수 있는가?


추상메서드 하나를 가지고 처리할 수 있었는가?

sql이라면,

`select * from jigak_history where date_jigak >= '2020-01-01' or date_jigak <='2021-12-31'` 
이렇게 되는데, or이니까 데이터 너무 많이나와


