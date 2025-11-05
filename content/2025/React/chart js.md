다 만들수가 없으니까 필요한것들은 우리가 원하는걸 설치해서 쓸 수 있다
이쁘게 누가 만들어놓은 것들을 쓰게 된다는 말이다

# npm에서 필요한걸 검색해라

npm사이트에서 
`react chart js`라고 검색을 해봐.
이름 비슷한거 엄청 많겠지만, 다운로드 수를 봐라.
다운로드 수가 많은것들이 안정적이고 좋다.

거기 들어가면 다 start어떻게 해야하는지 나온다.


## 설치하기

```sh
pnpm add react-chartjs-2 chart.js
# or
yarn add react-chartjs-2 chart.js
# or
npm i react-chartjs-2 chart.js
```



## example확인

docs안의 components 확인해라.

```js
'use client';

import Image from "next/image";  
import { Pie } from 'react-chartjs-2';  
  
  
  
export default function Home() {  
  return (  
    <div >  
        <Pie            options={  
              
            }  
            data={  
              
            }  
        />  
    </div>  
    );  
}
```

기본세팅만 이렇게 해놓고.



## gpt이용해서 data만들어 넣어라.

> nextjs프로젝트에서 chartjs를 연동하려고 패키지를 설치 다 했어. pie차트를 쓰고싶은데 목업데이터랑 옵션좀 넣어줘

라고 prompt를 작성해라. 
그리고 만들어준 것을 가지고 넣어봐라.
