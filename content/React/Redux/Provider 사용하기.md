
# 1. slice 에서,

행동 하나당 액션(reducers내부) 하나가 만들어지는거다.

```js
const menuSlice = createSlice({  
    name: "menu",  
    initialState,  
    reducers: {  
        setSelectMenu: (state, action) =>{  
            state.selectedMenu= action.payload;  
            state.menus.forEach(menu => {  
                menu.isActive = (menu.path === state.selectedMenu);  
            });  
        }  
    }  
})
```

위에서 setSelectMenu 자체가 액션이다 이거야.
액션 하나당 하나씩 만드는거야.

없으면 액션 안넣어도돼.
***즉, 그냥 값만 보여주고 바꿀 필요가없으면 안넣어도 된다고.***

```js
const initialState = {  
    copyright: "© 2025 Yoondd. All rights reserved."  
};  
  
const commonSlice = createSlice({  
    name: "common",  
    initialState,  
    reducers: {}  
});
```

이렇게 말이야.



### 무얼관리할 것인가는 initialState안에 넣는거야

```js
const initialState = {  
    menus: [  
        { name: "Dashboard", path: "/", isActive: true },
        { name: "Test", path: "/test", isActive: false }  
    ],  
    selectedMenu: "/"  
}
```



### 왜 export가 두갠데?

```js
export const { setSelectMenu } = menuSlice.actions;  
export default menuSlice.reducer;
```

그냥 파일만 부르면 기본으로 menuSlice.reducer을 보내는거고,
수동으로 찝어서 setSelectMenu 함수도 가져갈 수 있다고 말해주는거야.

그러니까 export가 몇개든 상관이없어.
위에껀 action내보내는거고 아래껀 디폴드로 그냥 reducer내보내는거라구.


---


# 2. Store에 등록하기

당연히 reducer만들었으면 store에 몇개의 구역이 있고 관리자 누군지 알려줘야지.

```js
//store.js
import { configureStore } from '@reduxjs/toolkit';  
import menuReducer from './menuSlice';  
import commonReducer from "./commonSlice";  
  
export const store = configureStore({  
    reducer: {  
        menu: menuReducer,  
        common: commonReducer  
    }  
})
```

위 코드를 해석하면 가게에 두개의 구역이 있고, 
하나는 menu라는 코너, 하나는 common이라는 코너다.

```js
//menuSlice.js 참고로 넣었어. 이거보고 이해 잘 하라고.
const initialState = {  
    menus: [  
        { name: "Dashboard", path: "/", isActive: true },
        { name: "Test", path: "/test", isActive: false }  
    ],  
    selectedMenu: "/"  
}
```

값을 보기위해 가져갈 때는, 이렇게 하면돼.

***store.menu.menus***



---



# 3. Provider만들어주기

그야말로 제공자야. 
쿠팡을 구독해야만 뭔 갖다주듯이, 우리도 가게를 구독해야해. 그래야지만 가져다 주거든.

`src / app / providers.js` 를 만들어주란 말이야.


```js
//provider.js
"use client";  
  
import { Provider } from 'react-redux';  
import { store } from '@/redux/store';  
  
export function Providers({ children }){  
    return <Provider store={store}>  
        {children}  
    </Provider>  
}
```

자, 우리가 만든 provider 파일이야.
Provider는 store를 배달해주는 애야. 그런애를 내가 지금 만들었어.

***Provider에다가 store를 넣었어. 
이렇게 Provider 범위 내에 있어야만 children이 Provider가 가져다 주는 것들을 사용할 수 있어.***



---

# 4. layout에서 불러주기

만든 provider를 어디서 불러야 가장 효율적으로 사용할 수 있을까?
어디서나 store의 값을 이용하려면 layout에서 불러주는게 제일 효율적이란말이야.

```js
"use client";

import { Providers } from './providers';
import { useSelector } from "react-redux";
import Menu from '@/components/Menu';

function MainContent({ children }){
    const copyright = useSelector(state => state.common.copyright);
    return (
        <>
            <div>
                <Menu />
                {children}
            </div>
            <footer>
                <p>{copyright}</p>
            </footer>
        </>
    );
}

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Providers>
            <MainContent>{ children }</MainContent>
        </Providers>
      </body>
    </html>
  );
}

```

일단 import해서 내가만든 provider를 불러와.
그게 일단 기본이지.

```js
        <Providers>
            <MainContent>{ children }</MainContent>
        </Providers>
```

이 부분을 주목해봐.


#### Providers로 감싼건 쉬운데, MainContent를 왜 감싼건지알아?

useSelect를 사용할거거든? 그건 훅(Hook)이야.
그런데 실행시점이 달라. 훅은 런타임때 실행이 되거든. 
Provider는 미리 구독안하면 안되거든?  그래서 실행시점이 안맞아.
서버가 실행되기전에 먼저 Provider를 구독하고있어야해.

그러니까 이렇게 할 수 없다는거야.

```js
export default function RootLayout({ children }) {
    const copyright = useSelector(state => state.common.copyright);
    return (
    <html lang="en">
      <body>
        <Providers>
            <div>
                <Menu />
                {children}
            </div>
            <footer>
                <p>{copyright}</p>
            </footer>
        </Providers>
      </body>
    </html>
  );
```

위에 이거 절대 안된다는 말이야.
왜 안되냐. 
==Providers를 return하기전에 useSelector에 copyright의 값을 담을 수가 없다고.==

꽤 어려웠는데 계속 보니까 이게 이해됐어.
return되는 순간에 Provider를 구독하는건데,
그전에 먼저 copyright가져다달라고? 
그런 말같지도 않은 상황이 어딨어.

그러니까 순서를 맞춰줘야지.





