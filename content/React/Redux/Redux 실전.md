

## 1. redux폴더 만들기
app폴더 말고, 소스가 모인 src폴더에 redux 디렉토리 만들기
우리가 새로 설치한거니까 아예 따로 관리하기위해서 만드는거야.

---

## 2. 큰 가게 만들기 - store.js

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

## 3. 가게 내부의 코너 만들기 - createSlice

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

## 4. Store에다가 알려주기

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

## 5. reducer의 등장

옛날에, toolkit을 사용하기 이전에는 3명이 따로 있었어
getter, setter(mutation-돌연변이), action(행동)
가져다 주는 애(리드온리), 넣어주는 애, 행동자가 3개를 받아서 세터를 호출해준다
이렇게 작업을 했었지.(redux순정말이야)
근데 이게 짱 불편하고 코드가 너무 많아지는거야.

어차피 setter와 action이 값을 수정하려는 목적이 동일하니까
reducer라는 하나로 퉁치자는 말이 나온거야
"두 개의 파이프를 연결하기위한 피팅"이라고 생각하면돼. 


#### menuSlice 사람만들자 

```js
//menuSlice.js
const menuSlice = createSlice({  
    name: "menu",  //슬라이스 이름 지어주기. 알아보기 쉽게 지어라. menuSlice에서 Slice떼고.
    initialState,  //이 구역에서 관리할 값이 뭐야? 바로 이거야. 위에서 만든거.
    reducers: {  //자 이제 사람 배치해보자!!
        setSelectMenu: (state, action) =>{  // 사람이 하는 일도 지정하자
            state.selectedMenu= action.payload;  
            state.menus.forEach(menu => {  
                menu.isActive = (menu.path === state.selectedMenu)  
            });  
        }  
    }  
})
```

결국 나가는애는 ***menuSlice***야. 알아보기쉽게 이름은 똑같이 지어버려.
이 구역에 대해 설정하는 Slice의 기능을 진행하는 아이다

1. 슬라이스 이름 쉽게 지어주기

2. 관리할 값 알려주기

3. reducers만들어주기
	바로 이게 관리할 사람인데.
	원래 우리는 getter, setter, action만들어야하는게 맞아.
	근데 귀찮으니까 누구한테 부탁하는거야?- toolkit이지!

4. setSelectMenu라는 '선택된 메뉴' 관리해주는 사람을 만들어주자. 
	액션은 한번에 일어나는거니까 필요한것들을 여기다가 다 넣어주는거야.
	두개처럼 보이지만 어차피 한놈이 하면되는 일이거든.
	할 일을 정해주면되는거니까 "익명함수"로 지정해준다.
	
	파라미터: 값, 액션
	`state.selectedMenu = action.payload` 를 살펴보면 action이라는게 나오는데, 바로 행위자를 뜻한다. 세팅할 값을 누가 가지고있냐면 바로 action이라는 행위자가 가지고 있는거다. 
	그리고 payload라는 개념을 알아볼까? 이건 따로 정리했다 - [[payload]]
	


#### export로 뽑아주자

```js
export const { setSelectMenu } = menuSlice.actions;  
export default menuSlice.reducer;
```

{}으로 묶은이유는 내보낼 메서드만 고르라는거다
자동으로 reducer같은걸 만들어주다
결국 위쪽에서 만들어낸 return값이라고 볼 수 있다.
결과물이다.

---


## 6. 이제 진짜로 store에 세팅해보자

```js
//store.js
import { configureStore } from '@reduxjs/toolkit';  
import menuReducer from './menuSlice';  
import memberReducer from './memberSlice';  
  
export const store = configureStore({  
    reducer: {  
        menu: menuReducer,  
        member: memberReducer  
    }  
})
```

reducer을 구독하는 형식으로 사용하는거다.
구독하면 provider가 원하는 내용을 제공하는것이다.

앞으로 값을 수정하려면 해당 slice의 reducer에게 부탁해야하는거다 (action - 행동하는 사람)
액션배우를 따라다니는 디스패치가 있다. dispatch가 따라다닌다.
