```sh
npx create-next-app@latest nextjs1 --typescript
```

저기서 nextjs1은 폴더명이다.
이렇게 할 경우 여러가지 세팅이 나오는데,

```sh
✔ Would you like to use ESLint? … Yes
✔ Would you like to use Tailwind CSS? … No
✔ Would you like your code inside a `src/` directory? … Yes
✔ Would you like to use App Router? (recommended) … No
✔ Would you like to use Turbopack for `next dev`? … No
✔ Would you like to customize the import alias (`@/*` by default)? … No
```

설치가 다 되면, 해당 폴더로 이동하고

```sh
cd nextjs1 #해당폴더로 이동
```



자, 이제 폴더 내에서 필요한 것들을 설치해보자

```sh
npm install sass
```

폴더 구조를 본다면
src내부에 pages에는 컴포넌트
styles엔 css가 들어간다.

그리고 pages내부에 api가 있는데, 서버관리를 한다는걸 알수가있다. 그 외의 모든 파일은 api폴더 바깥에 있다.



 기존 리액트는 app이 총괄대장이었는데 nextjs 에서는 _ app.tsx가  딱히 총괄이 아닌거같다.


## 실행하기

잘 설치되었는지, 한번 바꿔본 scss파일이 잘 되는지 

```sh
npm run dev #실행하기
```


---


## db연동을 위해 설치

gcp에 연동시켜서 sql에 테이블 만들어서
로그인 로그아웃 관리 테이블 만들거고
database 연동을 위해 

```sh
npm install mysql2 # db설계
npm install bcrypt # 비밀번호를 암호화 해주는 친구. 암호화. 해시
```

bcrypt는 타입스크립트를 지원하지않으니 타입스크립트 지원을 한번더 설치해야한다

```sh
npm install -save-dev @types/bcrypt
```

얘를 안하면 bcrypt를 인지하지 못하므로 당연히 설치해야한다.



```sh
npm install axios
```


## 로그인 세션에 활용하기위해 설치

JWT도 알아보자. Json Web Token - 로그인에서 세션할때, JWT가 세상쉽게 토큰도 만들어주고, 다른거 신경안쓰고 서비스를 좋게 만들어준다. 

로그인 후에 서버로부터 받은 토큰을 가지고, 그다음부터 자료요청할때 이 토큰을 같이 붙여서 요청한다. 그걸 보고 서버가 *오, 로그인 한 사용자구나* 라고 생각해서 자료를 준다. 사용자의 로컬스토리지에 가지고있다가 뭐 요청할때 서버로 붙여 보낸다.


```sh
npm install jsonwebtoken
npm install -save-dev @types/jsonwebtoken
```

얘도 타입스크립트를 지원하지 않기때문에 추가설치 필요.



쿠키안에 jwt를 집어넣으면, 서버와 통신할때 번거롭지않도 편안하다 - 자동으로 서버랑 주고받고가 가능하다.
웹브라우저에는 쿠키라는 공간이 따로 있고, 웹브라우저의 모든 정보가 담겨있다(헤더에있는 정보 등)

```sh
npm install cookie # 쿠키는원래있지만 거기에 jwt넣을거얌
npm install -save-dev @types/cookie # 타입스크립트 이해해랏!
```

