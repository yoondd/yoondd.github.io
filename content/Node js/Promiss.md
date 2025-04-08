비동기에서 콜백지옥에서 벗어나기위해 사용하는 객체로, 실행은 하지만 결과값은 나중에 받도록 설정하는 객체를 말한다.

```js
promise
	.then((msg1)=>{
		return new Promise((resolve, reject)=>{
			resolve(msg1);
		});
	})
	.then((msg2)=>{
		return new Promise((resolve, reject)=>{
			resolve(msg2);
		});
	})
	.then((msg3)=>{
		console.log(msg3);
	})
	.catch((err)=>{
		console.log(err);
	})
```

위와같은 형식으로 이용할 수 있다.
비동기가 기본인 자바스크립트에서 동기처럼 사용하고 싶어서 쓰는 객체라고 생각하면된다.

일단 실행을하고, 결과값은 실행이 완료된 후에 then이나 catch에서 받는다.
then이 수행이 되면 값은 다음의  then으로 이어지고 혹시라도 중간에 오류가 나타나면 catch가 수행된다고 볼 수 있다. 

---


비동기 작업을 처리하기위한 객체인데,
작업의 성공이나 실패를 나타내며 비동기코드를 보다 동기처럼 체계적으로 관리할 수 있다

```js
const myPromise = new Promise((resolve, reject) => {
  // 비동기 작업 수행
  const data = fetch('서버로부터 요청할 URL');
  
  if (data) {
    resolve(data); // 작업 성공 시 호출
  } else {
    reject("Error"); // 작업 실패 시 호출
  }
});

```

보통 promise는 보통 이렇게 두개의 매개변수를 받는다. resolve와 reject말이다.
then()은 성공했을때 실행할 것
catch()는 실패했을때 실행할 것
finally()는 성공여부와 상관없이 항상 실행되는 것이라고 볼 수 있다.

```js
myPromise
  .then(result => console.log("Success:", result))
  .catch(error => console.error("Error:", error))
  .finally(() => console.log("Completed"));

```


## promise의 장점

- 가독성이 매우 좋다
- then()을 통해 체이닝으로 순차적 처리가 가능하다. 따라서 복잡한 비동기 로직을 간결하게 구현할 수 있다.
- 에러를 한곳에 모아서 처리할 수 있다   (*콜백형태로 하면 에러처리가 불가능하다*)
- promise.all()을 이용하면 비동기 작업을 병렬로 진행할 수 있다
- 특정 상황에서 비동기 작업을 취소할 수 있다


## promise의 단점

- 에러처리가 너무 복잡하다
- fail-fast동작(  promise.all()은 전달된 promise 중에서 하나라도 실패하면 전체실패로 생각한다) 



---


Blocking model = Synchronous processing model
non-blocking model = Asynchronous processing model

```js
// Promise 객체의 생성 
const promise = new Promise((resolve, reject) => { // 비동기 작업을 수행한다. 
	if (/* 비동기 작업 수행 성공 */) { 
		resolve('result'); 
	} else { /* 비동기 작업 수행 실패 */ 
		reject('failure reason'); 
	} 
});
```

위가 promise 객체의 모습이다



## promise의 상태

| 상태            | 의미                        | 구현                                  |
| ------------- | ------------------------- | ----------------------------------- |
| pending       | 비동기 처리가 아직 수행되지 않은 상태     | resolve 또는 reject 함수가 아직 호출되지 않은 상태 |
| **fulfilled** | 비동기 처리가 수행된 상태 (성공)       | resolve 함수가 호출된 상태                  |
| **rejected**  | 비동기 처리가 수행된 상태 (실패)       | reject 함수가 호출된 상태                   |
| settled       | 비동기 처리가 수행된 상태 (성공 또는 실패) | resolve 또는 reject 함수가 호출된 상태        |

promise는 위와같은 상태를 가질 수 있다. 대기/성공/실패/성공또는실패 이라고 볼 수 있다


## promise 예제

```js
const promiseAjax = (method, url, payload) => {
	return new Promise((resolve, reject) => {
		const xhr = new XMLHttpRequest(); //비동기 객체 만들기
		xhr.open(method, url);
		xhr.setRequestHeader('Content-type', 'application/json'); //json 날릴때 json이라고 규정해주기
		xhr.send(JSON.stringify(payload)); // payload가 제이슨이겠지.

		xhr.onreadystatechange = function () {
		// 서버 응답 완료가 아니면 무시
		if (xhr.readyState !== XMLHttpRequest.DONE) return;

		if (xhr.status >= 200 && xhr.status < 400) {
			// resolve 메소드를 호출하면서 처리 결과를 전달
			resolve(xhr.response); // Success!
		} else {
			// reject 메소드를 호출하면서 에러 메시지를 전달
			reject(new Error(xhr.status)); // Failed...
		}
		};
	});
};

promiseAjax('GET', 'http://localhost:4000/data', null)
	.then(response => {
		console.log('GET Success:', response);
	})
	.catch(error => {
		console.error('GET Error:', error);
	});
```
