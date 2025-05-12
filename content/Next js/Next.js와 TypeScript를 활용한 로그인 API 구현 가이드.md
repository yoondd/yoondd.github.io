수업일자: 2025-05-12 
# login.ts  - api 만들기

## 기본 import

```tsx
import type { NextApiRequest, NextApiResponse } from 'next'; // api route사용을 위한 필수
import { db } from '@/lib/db';  //db정보를 가져온다
import bcrypt from 'bcrypt';  // 입력된 비밀번호를 암호화하여 비밀번호 일치여부 확인을 위해 쓴다.
```

여기서 첫 줄에 필수라고 적어놨는데, 이게 필수인 이유는 - **Next.js에서 API Route를 만들 때 요청(req)과 응답(res) 객체에 타입을 명확하게 지정해주기 위해서**이다. 또한, Next.js 공식 문서에서도 API Route를 만들 때 항상 `NextApiRequest`와 `NextApiResponse` 타입을 import해서 사용하도록 예시와 함께 안내하고 있다. 이는 Next.js가 제공하는 API Route의 표준적인 사용법이라고 할 수 있다.


## 로그인 후 토큰관리를 위한 import

```tsx
//로그인정보를 쿠키에게 던져주고, jwt를 이용할거다 / 여기서부턴 로그인후에 진행할것들  
import jwt from 'jsonwebtoken';  //토큰을 만들기 위해 쓴다.
import { serialize } from 'cookie'; //웹브라우저가 알아들을 수 있는 문자열로 보내주겠다  
```


## 실제 로그인 기능을 구현할 함수 만들기

```tsx

const loginfunction = async (req: NextApiRequest, res: NextApiResponse) => {  
	//여기 내부에 이제 기능을 순차적으로 넣을 것이다.
}  
export default loginfunction;
```

이렇게 함수를 하나 딱. 만들어놓고, 순서대로 차근차근 기능을 구현해볼까?


### 1) method부터 확인하고 아니라면 날려주기.

```tsx
if(req.method !== 'POST') return res.status(405).json({ error: "Method not allowed" });  
```

### 2) 값 받아오기
POST 요청이 맞았다면 비로소 사용자가 보낸 요청의 값을 확인해야겠다

```tsx
//값 받아오기  
const { email, password } = req.body;  
```

### 3) 둘 중에 하나라도 값을 입력하지 않았다면 에러라고 하기

```tsx
if(!email || !password) return res.status(400).json({ error: "Please enter data" }); 
```


### 4) 모든 값을 잘 입력했는지 확인했다면, 드디어 db에서 값 가져오기

```tsx
try {  
  //자 이 내부에 또 차근차근 일을 시작해볼까?
} catch(err){  
	console.error(err);  
}  
```

#### (1) 값 가져오기

```tsx
const [ data ] = await db.query(`SELECT * FROM users WHERE email = ?`, [email]); 
```

#### (2) 이메일이 없다면 리턴시키기

```tsx
//이메일이 없다면 가입이 안된거니까 날리기  
const user = ( data as any[] )[0]; // 타입스크립트가 못알아들으니까 as any[] . 있냐없냐만 확인할거니까 [0]체크하자  
if (!user) return res.status(401).json({ error: "User not found" }); 
```

#### (3) 이메일이 존재하나 비밀번호가 틀렸다면 또 리턴시키기

```tsx
//입력된 비밀번호(password)와 user의 정보에있는 password가 같은지를 확인한다  bcrypt로 비교하기 
const checkPassword = await bcrypt.compare(password, user.password);  

//비밀번호가 틀린경우 날리기  
if( !checkPassword) return res.status(401).json({ error: "Invalid password" });  
```


#### (4) *로그인되었구나!!* 이제 토근 만들기

```tsx
//이제 jwt로 토큰만들기 <토큰생성>  
//sign은 토큰 생성하는데, - sign(사용자정보, secret키(env에 넣은것), 만료시간(30m, 1h, 3d...))  
const token = jwt.sign(  
	{  
		id: user.id,  
		email: user.email,  
		name: user.name,  
	}, //토큰에 포함된 구체적인 사용자정보를 우리는 페이로드라고한다. Payload  
	process.env.JWT_SECRET!, //nextjs에게 알려준다 - 얘는 무조건있어 무조건!! 이뜻때문에 !를 붙여준다.  
	{ expiresIn: '1h' }  
);  
```


#### (5) 웹브라우저의 헤더에 토큰 붙여주기

```tsx
//그 토큰을 쿠키에게 던져주기  
//serialize('쿠키이름', '쿠키의값', {옵션});  
const cookie = serialize('token', token, {  
	httpOnly: true,  
	path: '/',  
	maxAge: 60*60,//쿠키의 유효시간. 무조건 초가 들어가야한다. 3일이고싶으면 3*24*60*60        });  

// 저 쿠키는 웹브라우저의 헤더에 들어간다  
// setHeader(예약된헤더이름, 헤더값)  
// setHeader('Content-Type', 'application/json');이런거처럼...  
res.setHeader('Set-Cookie', cookie);  
```


#### (6) 잘됐다고 응답주기

```tsx
// 성공했다고보내주기  
res.status(200).json({message: "Login Success"});  
```



자, 여기까지 예쁘게 서버만들었으니 이제는 page로 넘어가볼까~?

[[로그인 폼 구현하기]]

다시 말하지만 nextjs의 api route는 너무 좋아서, 기본 리액트처럼 따로 써먹을필요가없이
하나로 다 퉁칠수가있어. 전부 api에 들어가니까!