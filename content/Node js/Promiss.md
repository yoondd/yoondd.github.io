비동기에서 콜백지옥에서 벗어나기위해 사용하는 객체로, 실행은 하지만 결과값은 나중에 받도록 설정하는 객체를 말한다.

promise라는 것 자체가 ==비동기 함수가 반환하는 객체==라고 할 수 있다


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


---
# 이상하게 생긴 문법 공부하기

```js
resolvedPromise.then(console.log);

resolvedPromise.then(res => { console.log(res); });
```

공부를 하다보니, 이 두 코드가 동일하게 진행되는 것을 알 수 있었다. 
어떻게 이것이 가능할까? 알아본 결과 다음과 같다.

첫번째 코드에서 console.log라는 이름만 전달이 되었으나, javascript는 이를 콜백으로 처리했다. promise가 완료되면 결과값이 자동으로 console.log의 첫번째 인자로 넘어가게 되는 것이다. 왜냐고? 자바스크립트의 promise시스템은 이를 자동으로 호출시점에 결과값을 인자로 전달하기 때문이다. 두번째처럼 굳이 명시하지않아도 진행이 된다. ==그냥 콜백자리에 함수이름을 넣어 자동으로 인자가 쏙 들어가게 한다==

두번째 코드에서는 명시적으로 res를 받아와서 console.log(res)라고 실행했다. 이는 위와 동일하다. 



### 이를 다른 코드에서 확인해볼까?

```js
const numbers = [1, 2, 3]; // 명시적으로 선언한 화살표 함수 

numbers.forEach(num => console.log(num)); // 동일한 동작을 하는 함수 이름 전달 방식

numbers.forEach(console.log);
```

이렇게 간단하게 적을 수 있다는 것이다.
이렇게 할 수 있는 조건은 두가지가 있는데,

1. 콜백 함수가 인자를 그대로 받아들이는 경우. 
2. 추가 로직이 없이 그냥 이거밖에 없는 경우.

두가지만 가능하다. 




#  Promise.all

프로미스가 담긴 배열 등을 이터러블 인자로 전달 받는다. 
전달받은 모든 프로미스를 병렬처리하고 처리결과를 resolve하는 새로운 프로미스를 반환한다.

## all을 이용한  resolve의 순서

```js
Promise.all([
	new Promise(resolve => setTimeout(() => resolve(1), 5000)), // 1
	new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
	new Promise(resolve => setTimeout(() => resolve(3), 1000)) // 3
]).then(console.log) // [ 1, 2, 3 ]
.catch(console.log);
```

위 코드를 보면 사실 비동기의 경우 3이 가장 먼저 실행되어야하지만, 그렇지 않다. 동기적으로 진행이 되기때문에 첫줄부터 시작하고 있고, 순차적으로 배열이 차곡차곡 담아서 then한테 보내는 것을 볼 수 있다.

## 실패는 어떨까?

```js
Promise.all([
	new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 1!')), 3000)),
	new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 2!')), 2000)),
	new Promise((resolve, reject) => setTimeout(() => reject(new Error('Error 3!')), 1000))
]).then(console.log)
.catch(console.log); // Error: Error 3!
```

실패는 일단 발생한놈이 우선이다. 따라서 위에코드를 보면 error1이 출력되야 정상이지만, 위 코드의 결과를 보면 가장 빠른시간내에 발생한 error3이 확인된다.




---

# Promise.race

all이랑 비슷하게 프로미스가 담겨있는 배열을 인자로 받아서 병렬처리하지만
순서대로 하지않고 가장 먼저 처리된 프로미스가  resolve하는 새로운 프로미스를 반환한다. 

```js
Promise.race([
	new Promise( resolve => setTimeout(()=>resolve(1), 3000) ),
	new Promise( resolve => setTimeout(()=>resolve(2), 2000) ),
	new Promise( resolve => setTimeout(()=>resolve(3), 1000) )
])
.then(console.log)
.catch(console.log)
```

에러가 발생한 상황에서는 all과 동일하게 동작한다.  그냥 제일빨리 일어난  reject를 반환한다는 소리다.




[[json() 메서드]]