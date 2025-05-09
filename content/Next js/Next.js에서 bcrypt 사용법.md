## bcrypt가 무엇인가?

bcrypt는 비밀번호 같은 중요한 정보를 안전하게 저장할 때 쓰는 해시 함수 라이브러리야.  
비밀번호를 그냥 저장하면 위험하니까, bcrypt로 암호화해서 DB에 저장하는 거지.  
복호화(원래대로 되돌리기)가 안 되는 **단방향 암호화**라서 보안에 좋아.

==next js에서 꼭 await랑 같이 쓰자==

## bcrypt의 특징

- **단방향 해시**: 암호화된 비밀번호는 다시 원래대로 못 돌려.
- **Salt(솔트) 추가**: 같은 비밀번호여도 해시값이 매번 달라져서 해킹이 더 어려워져.
- **해시 반복 횟수**: 반복 횟수(saltRounds)를 늘릴수록 보안이 더 세지지만, 속도는 느려져.

## 설치

```sh
npm install bcrypt # 또는 환경에 따라 npm install bcryptjs
```


## 비밀번호 암호화(해싱)

```js
import bcrypt from 'bcrypt'; 
const saltRounds = 10; 
const hashedPassword = await bcrypt.hash(plainPassword, saltRounds); // hashedPassword를 DB에 저장하면 돼
```


## 비밀번호 검증(비교)

```js
const isMatch = await bcrypt.compare(inputPassword, hashedPassword); // isMatch가 true면 비밀번호가 맞는 거야
```


## Next.js에서 주의할 점

- bcrypt는 **서버(Node.js)에서만** 쓸 수 있어. 클라이언트(브라우저)에서는 안 돼.
- 비동기 함수라서 `await`를 꼭 써야 해.


## 요약

- bcrypt는 비밀번호를 안전하게 암호화해서 저장하고, 로그인할 때 입력한 비밀번호랑 비교하는 데 쓰는 라이브러리야.
- Next.js에서는 API Route나 서버 컴포넌트에서만 써야 해.
- 보안 신경 쓸 땐 bcrypt가 필수


---


### 그럼 bcrypt는 nextjs에서만 쓸 수있는 라이브러리인가?

아-니, bcrypt는 Next.js에서만 쓸 수 있는 라이브러리가 아니야.  
bcrypt는 Node.js 환경에서 동작하는 일반적인 암호화(해시) 라이브러리라서,  
Express, NestJS, Koa 같은 다른 Node.js 백엔드 프레임워크나 순수 Node.js 프로젝트에서도 똑같이 쓸 수 있어.

단, bcrypt는 서버(백엔드)에서만 동작하고, 브라우저(클라이언트)에서는 쓸 수 없어.  
Next.js에서 bcrypt를 쓰는 건, Next.js가 서버 기능(API Route 등)을 제공하기 때문이지,  
bcrypt 자체가 Next.js 전용은 아니야.

정리하면,  
**bcrypt는 Node.js 서버 환경에서라면 어디서나 쓸 수 있는 라이브러리**야!




#### 그럼 비밀번호가 맞았는지 틀렸는지는 서버가 비교해야겠네?

당연하지..!!! 비밀번호 비교는 서버에서 bcrypt.compare로 처리하고 프론트엔드는 절대로 비밀번호 해시값을 다루면 안돼!!!!!!!!!!!!!!!!!!!!
