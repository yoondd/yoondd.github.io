자, 이제 back작업을 해볼까?
next js의 가장큰 특징이다
기존에 node js를 이용했을때는  server js라는 파일이 필요했는데, 이제는 그런거 필요없다. 
api발급받기위해 지원해주는 기능이 많다.

1. api폴더에 signup.ts라는걸 만들어준다
2. 아주 필수가 되는 것들을 불러와야한다
	`import type {  NextApiRequest, NextApiResponse } from 'next';`
3. db연결을 위한 세팅도한다
	`import { db } from '@/lib/db';` 
	기 설계해둔 db파일을 가져오는 것이다
4. 암호화를 위해 bcrypt도 불러온다
	 `import bcrypt from 'bcrypt';`
5. 이제 async함수를 만들어보자
	`const backSignFunction = async (req: NextApiRequest, res: NextApiResponse) => {} `
	1)  insert작업이 아니라면 내보내는 내용을 먼저 적어주자,
	2) body에서 값 가져오기 - email, password, name
	3) 데이터가 비어있는 경우 대응하기
	4) try-catch문을 이용해서 에러가 났을떄 오류가 뜨도록 일단 만들어주기
	5) try부분에 만들어주기
		- 이미 등록된 이메일인지 확인하기
		- 패스워드 암호화하기
		- insert into 작성하기
		- 모든 작업 후에 보낼 상태정보 등록하기(201)


### 코드전문

```ts
import type {  NextApiRequest, NextApiResponse } from 'next';  
import { db } from '@/lib/db';  
import bcrypt from 'bcrypt';  
  
const backSignFunction = async (req: NextApiRequest, res: NextApiResponse) => {  
  
    // post 작업이 아니라면 나가버려. 왜나가는지 "허용안되는 메서드"라는 것도 보여주고 말야.  
    if(req.method != 'POST') return res.status(405).json({ error: "Method Not Allowed" });  
  
    // 테이블 항목명이랑 이제 잘 맞춰서. 받아오는것 세팅.  
    const { email, password, name } = req.body;  
  
    // 데이터가 비어있는 경우 대응.  
    if( !email ||  !password || !name ) {  
        return res.status(400).json({ error: "Please enter data" });  
    }  
  
  
    try {  
        // 원래 이렇게 하잖아??  
        // const rel = await db.query();        // const data = rel[0]  
        // 아래처럼 쉽게 쓸 수 있다. - 하고픈일: 이메일이 같은지를 묻고싶다.  
        const [ data ] = await db.query(`SELECT id FROM users WHERE email = ?`, [email]);  
        // 여기서 data에 값이 있으면 누군가 그 이메일로 가입을 했다는 사실이 된다.  
        if( (data as any[]).length > 0 ) {  
            return res.status(400).json({ error: "Email already exist" });  
        }  
  
        // 걸러내는걸 다 했으니까 정상적인 가입절차를 진행한다  
        // 오? 근데 패스워드를 그냥 넣어주면 큰일나겠구나! 드디어 bcrypt를 쓸 때구나.  
        const hashPassword = await bcrypt.hash(password, 10); // 10은 복잡단계야  
  
        //이제 받은 자료를 테이블에 넣기만 하면되겠다  
        //created at 은 생성일이 자동으로 들어가게끔 세팅이 되어있다.  
        await db.query('INSERT INTO users ( email, password, name ) values (?,?,?) ', [email, hashPassword, name]);  
  
        //여기까지 나와있는 json()은 전부 나를 위한거고 고객에게 보여줄 것들은 프론트에서 하는 것이다  
        res.status(201).json({ success: '회원가입 성공' });  
  
    } catch(err) {  
        console.error(err);  
        res.status(500).json({ error: "Internal Server Error" });  
    }  
  
  
}  
  
  
export default backSignFunction;
```


여기까지 했으면 이제  front세팅으로 가볼까?

<<<<<<< HEAD
[[front setting -  회원가입 tsx]]
=======
[[Next.js 회원가입 폼 구현과 GCP DB 연동 실습]]
>>>>>>> origin/main
