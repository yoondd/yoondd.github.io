감시할 필요가 있을 때, 붙인다.
그래야만 감시자가 붙어서 쓸 수 있다

```js
"use client";  
  
import { Providers } from './providers';  
import { useSelector } from "react-redux";  
import Menu from '@/components/Menu';
```

사용자 쪽에서 쓰는 기능이라는 뜻이다.
프론트에서 보일 페이지라는 이야기다.


`useSelector`같이 use가 붙은 것은 전부 hook인데,
이런 훅은 *런타임*시점에 실행이 된다.
그러려면 감시자가 파견되어서 이벤트를 확인해야한다.
감시자를 내보내야하니까.
버튼같은거 감지하려면 어쩔 수가 없다.
이벤트 감지 등 감시해야할 일이 있으면 use client를 써줘야한다.



### 반대로 그럼 언제 안써야해?
이벤트 감지할 필요가없고, 
데이터를 가공하거나 데이터를 가공해서 앞단의 페이지로 보낼 때. 그럴때는 사용하지않는다.

예를들어서 api콜 같은걸 해서 데이터를 받아올거잖아? 
이건 보이면 안된다는거지. 감시할 필요가 없는 일회용이야.
backend라고 하긴 좀 뭐해도 뒤에서 몰래 작업하는 데이터 작업을 할 때는
오히려 use client를 붙이면 안된다
계속 감시하니까 메모리 누수가 일어나겠지.
그래서 절대 안된다.
