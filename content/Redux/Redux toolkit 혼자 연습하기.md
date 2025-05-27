리덕스 공부는 정말 너무나 어려웠지만, 애쓰며 해봤다.

## install

```sh
npm install react-redux @reduxjs/toolkit
```


## source tree

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── providers.js
│   └── gift/
│       ├── GiftPage.tsx
│       └── page.tsx
├── components/
│   ├── Friend.tsx
│   └── Items.tsx
├── store/
│   ├── friendSlice.ts
│   ├── historySlice.ts
│   ├── itemSlice.ts
│   └── store.ts
├── types/
│   └── Interface.ts
├── style/
│   ├── Friend.module.scss
│   └── ...
```



## `store.js`

```js
//store.js
import { configureStore } from "@reduxjs/toolkit";
import friendReducer from './friendSlice';
import itemReducer from './itemSlice';
import historyReducer from './historySlice';

export const store = configureStore({
    reducer: {
        friend: friendReducer,
        item: itemReducer,
        history: historyReducer
    },
});
```

처음부터 다 적었던 것은 아니다. 일단 기본세팅으로 `configureStore`만 불러오고 slice들을 작성한 후에 다른 것들을 추가로 적어냈다. 편의를 위해 그냥 다 적어 놓았을 뿐이다.



## `friendSlice.ts`

```js
//friendSlice.ts
import { createSlice } from '@reduxjs/toolkit'


const initialState = {
    list: [],
    selectedFriend: null,
}

const friendSlice = createSlice({
    name: "friend",
    initialState,
    reducers: {
        setFriendList: (state, action) => {
            state.list = action.payload;
        },
        setSelectedFriend: (state, action) => {
            state.selectedFriend = action.payload;
        }
    }

})

export const { setFriendList, setSelectedFriend } = friendSlice.actions;
export default friendSlice.reducer;
```

편의를 위해 역시 slice도 하나만 적어봤다. 

사실 나에겐 친구목록, 선물목록, 내역 데이터를 다루는 3가지의 슬라이스가 존재한다. 

개념만 확인하면되니까 하나만 올려봤다. 

이렇게 예쁘게 생성해두고 store에서 import하면 된단 소리다.




## `provider.js`

```js
// app/provider.js
'use client'

import { Provider as ReduxProvider } from 'react-redux';
import { store } from '@/store/store';

export default function Providers({ children }) {
    return (
        <ReduxProvider store={store}>
            {children}
        </ReduxProvider>
    );
}
```

나를 가장 고통에 밀어넣은 존재, provider다. 

이 친구는 제공자다.  구독하지않으면 제공해주지 않기때문에 구독을 위해 반드시 필요하다.




## `layout.tsx`

```tsx
import "./globals.css";
import Providers from "@/app/providers";

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
    return (
    <html lang="en">
      <body suppressHydrationWarning={true}>
          <Providers>{children}</Providers>
      </body>
    </html>
  );
}

```

여기서 provider를 구독한다. 값에 접근하려면 어쩔 수 없다.



## `app/gift/page.tsx`

```tsx
import GiftPage from "@/app/gift/GiftPage";

const Page = async () => {
    const friendRes = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/friend/all`);
    const itemRes = await fetch(`${process.env.NEXT_PUBLIC_API_URL}/item/all`);
    const friendData = await friendRes.json();
    const itemData = await itemRes.json();
    return (
        <GiftPage friends={friendData} items={itemData} />
    );
};

export default Page;
```

자, 여기서 잘 보면 fetch기능을 하는 것을 볼 수 있다. (참고로 api는 env에 따로 저장했다)

이렇게 이 페이지는 서버기능을 한다. 프론트 서버랄까.

여기서는 `'use client'`를 쓰지 않기때문에 값을 제어할 수 없어 prop으로 넘겨줄것이다.



## `app/gift/GiftPage.tsx`

```tsx
'use client'
import React from 'react';
import { useEffect } from 'react';
import { useSelector } from "react-redux";
import Friends from "@/components/Friends";
import Items from "@/components/Items";
import styles from "@/style/Gift.module.scss";
import { Friend, Item } from "@/types/Interface";
import { setFriendList } from '@/store/friendSlice';
import { setItemList } from '@/store/itemSlice';
import { addHistory } from "@/store/historySlice";


const GiftPage = ({ friends, items }: { friends: Friend[]; items: Item[] }) => {
    const selectedFriendId = useSelector((state: any) => state.friend.selectedFriend);
    const selectedItemId = useSelector((state: any) => state.item.selectedItem);

    const friendList = useSelector((state: any) => state.friend.list);
    const itemList = useSelector((state: any) => state.item.list);

    const selectedFriend = friends.find((f: Friend) => f.id === selectedFriendId);
    const selectedItem = items.find((i: Item) => i.id === selectedItemId);


    useEffect(() => {
        dispatch(setFriendList(friends));
        dispatch(setItemList(items));
    }, [dispatch, friends, items]);


    return (
        <div className={styles.container}>
            <div className={styles.selectContainer}>
                <Friends />
                <Items />
            </div>
            <button className={styles.addbtn}>Gift</button>
        </div>
    );
};

export default GiftPage;
```

실제로 값을 가져와서 redux에 넣는 아이다.

가져온 값을 최초에 redux에 업데이트를 해낸다.



## `components/Friend.tsx`

```tsx
'use client'

import styles from "@/style/Friend.module.scss";
import { Friend } from '@/types/Interface';
import { useSelector, useDispatch } from 'react-redux';
import { setSelectedFriend } from '@/store/friendSlice';

const Friends = () => {
    const friends = useSelector((state: any) => state.friend?.list || []);
    const selectedFriend = useSelector((state: any) => state.friend?.selectedFriend || null);
    const dispatch = useDispatch();

    return (
        <div className={styles.selectInner}>
            <h2>Select Friend</h2>

            <ul>
                {
                    friends.map((f : Friend) => (
                        <li key={f.id} className={`${styles.list} ${selectedFriend == f.id ? styles.selected : " "}`} id={`${f.id}`} onClick={()=>{
                            dispatch(setSelectedFriend(f.id));
                        }}>
                            <div className={styles.friendProfile}></div>
                            <span className={styles.friendName}>{f.name}</span>
                        </li>
                    ))
                }
            </ul>
        </div>
    );
};

export default Friends;
```

길고도 길었던 여정 끝에, 드디어 사용할 수 있게 되었다.



---

여기까지 진행하면, fetch로 가져온 데이터를 스토리지에 담아서 어디서나 사용할 수 있게 세팅할 수 있다.

하지만, 히스토리를 추가하거나 친구 추가, 선물 추가 등이 일어날때 어떻게 효과적으로 다뤄야하는지 - 그러니까 db따로, redux따로 하는게 맞는지, 그렇다면 어느시점에 어느컴포넌트에서 해야하는지 - 를 공부해야겠다.