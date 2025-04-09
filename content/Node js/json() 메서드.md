
자바스크립트에서 .json()메서드는 fetch API응답 객체에 포함된 메서드다.
서버에서 json형태로 받은 데이터를 자바스크립트 객체로 변환하는 역할을 한다
비동기적으로 동작하며,  promise를 반환한다.

1.  JSON 데이터를 변환한다
	서버에서 받은 응답데이터가 json형식이면 파싱이 불가능하다
	이를 자바스크립트 객체로 변환한다. 
	내부적으로 JSON.parse()라고 볼 수 있다

2. Promise 반환
	비동기 작업을 처리하기위해 Promise를 반환한다
	변환이 완료되면 Promise가 resolved된다. JavaScript객체를 반환한다.

주로 Fetch API와 함께 사용된다.
서버에서 데이터를 가지고와서 클라이언트쪽에서 처리할 때 유용하다


## JSON.parse()랑 뭐가달라?

Fetch API의 response객체에 내장되어있는 .json()은 응답 본문을 읽고 데이터를 파싱한다. 반면에 JSON.parse()는 문자열 형태의 JSON 데이터를 자바스크립트 객체로 직접 변환한다. 