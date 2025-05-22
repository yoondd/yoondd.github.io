

## redux폴더 만들기
app폴더 말고, 소스가 모인 src폴더에 redux 디렉토리 만들기
우리가 새로 설치한거니까 아예 따로 관리하기위해서 만드는거야.

---

## 큰 가게 만들기 - store.js

가게에 가면 코너가 나누어져있는데, 일단 그 전에 큰 가게가 필요하다
그래서 큰 가게를 만들어야한다

```js
import { configureStore } from '@reduxjs/toolkit';
```

configureStore라는 메서드를 가져올것이다.
메서드 가져올때는 {}이렇게 해서 가져오는거다.

```js
export const store = configureStore({  
      
})
```

가게는 열었지만 안에 아무것도 없는 상태다. 
그러니까 이제 나누어서 작업을 해야지.

***공간을 쪼개서 쓰다 나누어쓰다 slice를 이용할 때다***

---

## 가게 내부의 코너 만들기 - createSlice

이제 코너 하나씩하나씩 만들면된다. 
그러니까 redux폴더에 menuSlice.js와 memberSlide.js를 만들어서 각 코너를 만들어보자

#### menuSlice

```js
import { createSlice } from '@reduxjs/toolkit';
```

코너 안에 장도 들어가있잖아. 
예를들어 과자코너에도 오예스칸, 에이스칸...있는것처럼 말이다 
이런것들을 만들어줘야해

```js
const initialState = {  
      
}
```

틀은 고정이고 안에 들어가는 값은 언제든 바뀔 수 있다

```js
const initialState = {  
    menus: [  
        { name: "Dashboard", path: "/", isActive: true }, 
		{ name: "Test", path: "/test", isActive: false }  
    ],  
    selectedMenu: "/"  
}
```

isActive 는 해당내용에 불넣기위함이다. 해당메뉴에만 불 들어오라고..


#### memberSlice

```js
import { createSlice } from '@reduxjs/toolkit';  
  
const initialState = {  
    token: "",  
    name: "",  
    isLogin: false,  
}
```


이런식으로 코너를 두개를 만들었네.
과자코너랑 유제품코너. 이렇게 있듯이 두개를 만든거야.
근데 내가 만들었다는걸 아무도 모른다구.
store에다가 등록을 해줘야지.

---

## Store에다가 알려주기

근데 그냥 알려주면 알수가없어. export설정을 안했잖아.

```js
//store.js
import { configureStore } from '@reduxjs/toolkit';  
import './menuSlice';  
import './memberSlice';
```

그냥이렇게 불러온다고 다된게 아니라구.
export하지도않았는데 뭐가 있는줄알고 가져오겠냔말이야.

뭘 해야겠어? 여기서 나오는게 바로 provider야.
그 설정을 해서 값에 접근할 수 있게 해줘야겠어.


---
