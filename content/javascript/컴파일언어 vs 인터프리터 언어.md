영상일자: 2025-03-13
영상제목: JS 프로토타입 인터프리터 언어의 특징
공부일자: 2025-05-19

# 컴파일언어와 인터프리터 언어의 차이부터 알아보자

### 컴파일언어
소스작성 - 컴파일러가 컴파일 - 빌드 - 실행파일
컴파일러가 있기때문에 결과파일이 하나의 실행파일로 나오고, 사용자가 언어를 깔 필요는없다.
따라서 실행파일을 실행하기위해 컴파일러가(er. 컴파일 해주는 애다) 다시 필요가 없다

### 인터프리터언어
소스작성 - 인터프리터가 코드실행
소스코드를 돌리려면 항상 상시대기하는 ==통역사==가 있어야한다.
인터프리터 언어에서는 코드를 script라고 하는데,  그야말로 ==대본==을 말한다.
따라서 인터프리터(해설가, 통역가)가 항상 대기하는 프로그램이 있어야하고, 그 프로그램에게 소스코드를 주면서 읽어달라고 해야한다.

근데 문제는 ..
10년전에 러시아어를 배운 통역사가 있고 (러시아어 버전 v.10.0.1)
3개월 전에 러시아어를 배운 통역사가 있다 (러시아어 버전 v.25.11.1)
언어는 발전하고, 예전언어는 최근에 배운 사람은 이미 알고있다

js는 인터프리터 언어라서, 통역사가 사는 웹브라우저안에서만 실행이 가능하다
즉, 웹브라우저가 소스코드를 받아서 읽어준다
웹브라우저 안에 js 인터프리터가 살고있다
그 놈이 .js파일을 받아 읽어줄 수 있는데,..
그 놈이 너무 오래된 놈인경우를 생각해보자. 러시아어버전 v.10.0.1을 쓰고있는 할아버지 통역사처럼.

사람들이 웹 브라우저를 다운로드받을때, 읽을 수 있는 버전이 결정된다.
소스코드 작성할 때, 최신 문법으로 작성을 하면 오래된버전의 브라우저에서는 읽히지않는 문제가 생긴다.
-> 컴파일언어는 개발할때 버전이 의미가 있지만 인터프리터언어는 그렇지가 않다는 말이다

그래서 개발할때 2015년에 개발한 ES6를 기준으로 제작한다


#### 인터프리터가 언어를 해석할 수 있다? - 의 의미.
문장의 구조를 숙지하고 단어를 알아야한다.는 것을 말한다
기본적으로 단어를 알고있다는 말인데. 
ES7 인터프리터보다 ES13 인터프리터가 알고있는 단어가 더 많다.

***프로토타입:모형. 단어장***
인터프리터언어는 프로토타입을 가지고서 실시간으로 해석을 하기때문에 프로토타입 언어라고 말한다.


#### 함수랑 메서드의 차이점?
함수: 독립적으로 사용할 수 있는 사람
메서드: 클래스 내에서 일하는 사람

자바스크립트에도 클래스가 있고 메서드가 있기는 했지만 
클래스라는 개념이 무언가를 찍어내는 아이고, 그릇을 만드는데 쓰이는 아이다
그렇기때문에 컴파일언어에 더 잘어울린다.
진정한 의미의 클래스가 아니라, 프로토타입보면서 뒤에서 가내수공업을 해내는 아이일뿐이다.


