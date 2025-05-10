next js 덕분에 쉽게 서버 파일을 세팅했으니, 이번에는 실제로 front에서 값을 보내볼까?


## 상단 import

```tsx
import React, { useState } from 'react'; //useState: input clear
import { useRouter } from 'next/router';
import styles from '@/styles/Signup.module.scss';
```

#### useState를 가져온 이유
value에 들어가는 값들을 상태관리할거고, 성공/실패여부를 담는 메시지도 만들거거든. 나중에 다 처리하고나면 input를 비워주기도 할거니까 이건 상태관리하는게 좋을 것같아.

#### useRouter
이걸 가져온 이유는 X버튼을 눌렀을때 창이 꺼지는 개념이 아니라 다른 페이지로 이동하는 개념을 사용하려고하는거야. route처리를하면 다른 페이지(루트)로 이동할 수 있어.


---


## 변수만들기

```tsx
// 회원가입에 필요한 주요 값
const [ email, setEmail ] = useState('');
const [ password, setPassword ] = useState('');
const [ name, setName ] = useState('');

//회원가입 성공여부
const [ message, setMessage ] = useState('');

//팝업창이 뜨고 닫으면 홈으로 갔으면 좋겠다
const router = useRouter(); // 훅이라서 다른 변수에 한번 튕겨줘야한다
```

#### input 마다 상태를 만든다?
동적으로 input을 입력할때마다 value값을 바꾸기도 할거고, 나중에 res.ok(보내는데 성공)했을때 해당 내용을 비워주기도 할거거든. 그래서 input으로 전달하는 값은 리액트에서 보통 상태관리를 해.

#### 갑자기 왠 메시지?
회원가입에 성공했거나, 이미 이메일이 있어서 거절당했다거나, 특정필드가 없어서 회원가입이 정상적으로 이루어지지않았거나, 서버 문제로 작동되지않을때(catch상황) 메시지를 사용자에게 전달해야겠어. 그러려면 메시지가(존재할때만) 보내주는 창이 있으면 좋겠지? 그래서 상태를 관리하는게 좋을 것 같아

#### useRouter()
라우터훅을 사용하겠다는건데 이걸 해가지고 나중에 x버튼을 눌렀을때 루트로 이동하게끔 만들어줄거야.



---

## form이 submit되었을때 실행되는 함수

```tsx
const formSubmit = async(e: React.FormEvent) => {
	//폼의 이벤트 방지
	e.preventDefault();

	try {


		//보내는 작업에 대한 fetch  server파일찾아가야하는거다. 그래서 /api/signup
		const res = await fetch('/api/signup', {
			method: 'POST',
			headers: { 'Content-Type': 'application/json' },
			body: JSON.stringify({email, password, name})
		})
		const data = await res.json();

		//회원가입이 성공했을 때, 제어
		if(res.ok) {
			setMessage('Sign Up Successful!');
			setEmail('');
			setPassword('');
			setName('');
		}else{
			setMessage(data.error || 'Failed to submit!');
		}

		//회원가입이 실패했을 때, 제어

	} catch (err) {
		console.error(err);
		setMessage('서버오류: 서버가 고장났어요'); // 고객이 보는 곳
	}
}
```

#### 기본 이벤트 꺼버리위한 event가져오기
e.preventDefault()는 많이 해봐서 알고 있지만 이건 typescript를 준수하는 next js라는걸 잊지말자. e에다가 이게 무슨 변수인지 정의했어. `e: React.FormEvent` 바로 이 부분이지.

#### async 선언
이 함수 내부에서는 fetch를 이용해서 값을 가져올거거든? 
그러면 기본적으로 async를 무조건 사용해야해. `const formSubmit = () =>{}`이게 아니라, `const formSubmit = async() =>{}` 라는거야. 이렇게 작동을 해야지만 내부에 await를 쓸 수가 있어.

#### try-catch를이용한 데이터 가져오기
catch는 그냥 에러일 경우에 서버가 작동하지않는 다는것을 알려주기 위함인거고. 실제로 중요한건 바로 try구간이지. try구간을 좀더 딥하게 살펴보자고.

- fetch를 이용해서 데이터 가져오기 세팅: method와 headers, body가 있다는 것을 잊지말자. 우리가 보내줘야할 데이터는 **요청방식, 헤더, 바디**다. 요청방식은 겟이냐 포스트냐 뭐 그런거니까 간단한데, headers는 외워버려 `{ 'Content-Type': 'application/json' }` 라는 것을. 그리고 body같은 경우는 `JSON.stringify()`안에 객체형태로 넣어야한다는 걸 잊지말고!
- 그리고 fetch로 보냈을 때 가져오는 응답은 `const data = await res.json()`에 있을 것이야.
* 회원 가입에 성공했을때에는 기존의 value값들을 초기화하고 메시지에 기분좋은 메시지를 주면돼
* 실패했을때는 에러띄우지뭐.

[[Next.js에서 bcrypt 사용법]]


---


## return 

여기까지 모두 작업이 되었다면 이제 return 내부를 꾸며주면되는데, 이건 특별한게 없으니까 코드만 남긴다.


```tsx

return (  
	<div className={styles.container}>  

		{/*팝업의 배경이 될 영역*/}  
		<div className={styles.bg}></div>  

		{/*실제 회원가입 팝업*/}  
		<div className={styles.signup}>  
			{/*누르면 루트로 가라*/}  
			<button className={styles.close} onClick={()=> router.push('/'); }>X</button>  

			<h2>Sign Up</h2>  

			<form onSubmit={formSubmit}>  
				<input type="text" placeholder="name" value={name} onChange={ e => setName(e.target.value) } />  
				<input type="password" placeholder="password" value={password} onChange={ e => setPassword(e.target.value) } />  
				<input type="email" placeholder="email" value={email} onChange={ e => setEmail(e.target.value) } />  
				<button type="submit">SEND</button>  
			</form>                {  
				//메시지가 나올 공간 - 메시지가 있을때만 나와야한다.  
				message && <h3 className={styles.message}>{message}</h3>  
			}  

		</div>  

	</div>    
	);  

```



## gcp 에서 확인하기

이전에 gcp 에서 db를 만든게 있었다.
거기서 한번 확인해볼까?

[[GCP를 이용한 db세팅]]

1. gcp sql으로 들어가기
2. 클라우드 쉘 열기
3. `gcloud sql connect root --user=root `을 이용해서 접속하기
4. `use root` db선택하기
5. `show tables` 확인하기
6. `select * from users` 테이블확인하기
