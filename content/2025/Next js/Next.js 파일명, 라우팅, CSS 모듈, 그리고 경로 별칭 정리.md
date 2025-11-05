### App.tsx를 공부해볼까?

파일이름 자체가 라우터가 된다.
그래서 파일이름을 잘 지어야한다.
그리고 소문자로 작성해야한다 - 하나의 주소가 되기 때문에.


## import의 @

`import "@/styles/globals.scss";`에서 `@`는 보통 프로젝트 내에서 특정 경로를 간단하게 참조하기 위한 별칭(alias)이다. 예를 들어, `@`가 프로젝트의 `src` 폴더를 가리키도록 설정되어 있으면, `@/styles/globals.scss`는 `src/styles/globals.scss` 파일을 의미한다.

이 별칭은 보통 웹팩(webpack)이나 Next.js 같은 빌드 도구의 설정에서 alias로 지정해 사용한다. 이렇게 하면 상대경로나 절대경로를 길게 쓰지 않고도 쉽게 파일을 불러올 수 있어 편리하다.
즉, `@`는 프로젝트 루트나 `src` 폴더를 가리키는 경로 별칭(alias)로, 해당 위치에서부터 경로를 지정하는 의미다

```ts
import "@/styles/globals.scss";   //이부부말이다 src로 바로 찾는다
import type { AppProps } from "next/app";  
  
export default function App({ Component, pageProps }: AppProps) {  
  return <Component {...pageProps} />;  
}  
```


## css 모듈화 해서 가져오기

```tsx
import styles from '@/styles/NavBar.module.scss'
```

 실제 파일도 당연히 module이 붙어있다 `NavBar.module.scss`

```tsx
<div className={styles.navbar}>navbar</div>
```
 


## nextjs의 루트?

루트는 무조건 pages / index.tsx를 의미한다.

```tsx
<ul className={styles.menu}>  
    <li>        
	    <Link href="/">Home</Link> {/*index.tsx로 무조건 똑같다. 그렇게 약속이되어있다*/} 
    </li>  
</ul>
```



## pages폴더 기반 라우팅

```jsx
<li>  
    <Link href="/about">about</Link> {/*pages에 파일만 존재하면된다*/}  
</li>
```

이렇게되어있을때 pages폴더에 그저 about만 있으면된다.



## export하는 함수의 이름은 반드시 대문자로 해야한다

```tsx
import React from 'react';  
import styles from '@/styles/MainPage.module.scss';  
  
const Index = () => {   
    return (  
        <div>  
        </div>  
    );  
};  
  
export default Index;
```

*아, 그래서 자동완성을 하려고하니까 Index로 나왔구나! *


>그중에 index는 이유불문하고 무조건 메인이다. 그렇기때문에 이름이 꼭 Index가 아니어도 디는거다. 즉, 파일명과 함수명이 달라도 상관이없다.  어차피 파일단위로 생각하기때문에 아무 상관이없다. 

```tsx
//`MainPage.tsx`

import React from 'react';  
import styles from '@/styles/MainPage.module.scss';  
  
const MainPage = () => {  
    return (  
        <div>  
        </div>    
        );  
};  
  
export default MainPage;
```

