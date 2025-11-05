수업일자: 2025-5-14

이미 네이버 개발자도구에 어플리케이션을 생성했다는 전제하에 시작한다. 
그거 안했으면 그거부터
[[네이버 Developers의 이용하기(뉴스)]]

## 1. 가이드 확인
https://developers.naver.com/docs/serviceapi/search/news/news.md#%EB%89%B4%EC%8A%A4


#### 가이드에서 요청 url을 확인한다.

| 요청 URL                                          | 반환 형식 |
| ----------------------------------------------- | ----- |
| `https://openapi.naver.com/v1/search/news.xml`  | XML   |
| `https://openapi.naver.com/v1/search/news.json` | JSON  |
|                                                 |       |

#### 파라미터도 확인한다(쿼리)

| 파라미터    | 타입      | 필수 여부 | 설명                                                                         |
| ------- | ------- | ----- | -------------------------------------------------------------------------- |
| query   | String  | Y     | 검색어. UTF-8로 인코딩되어야 합니다.                                                    |
| display | Integer | N     | 한 번에 표시할 검색 결과 개수(기본값: 10, 최댓값: 100)                                       |
| start   | Integer | N     | 검색 시작 위치(기본값: 1, 최댓값: 1000)                                                |
| sort    | String  | N     | 검색 결과 정렬 방법  <br>- `sim`: 정확도순으로 내림차순 정렬(기본값)  <br>- `date`: 날짜순으로 내림차순 정렬 |


아하, 내가 이렇게 요청하면되는구나
`https://openapi.naver.com/v1/search/news.json?query=날씨&display=15&sort=date`


---

## 2. server파일생성

```ts
import { NextApiRequest, NextApiResponse } from 'next';  
const NewsFunction = async (req: NextApiRequest, res: NextApiResponse) => {  
      
}  
export default NewsFunction;
```

원래 공공 api는 이런게 필요없으나 네이버 뉴스의 보안등급이 높아서
그걸 감추기위해서 사용한다


#### 사용자가 입력한 내용 받아오기

```ts
const keywords = req.query.query || "뉴스";
```

#### naver 양식에 맞게 요청하면서 인코딩 추가 - encodeURIComponent

```ts
const response = await fetch(  
    `https://openapi.naver.com/v1/search/news.json?query=${encodeURIComponent(keywords as string)}&sort=date`  
);
```

검색어를 이상하게 쳐도 대응할 수 있게 만든다, 
정확히는 자바스크립트에서 문자열을 URI(Uniform Resource Identifier) 구성요소로 안전하게 변환하기 위해 사용하는 함수로, 한글이나 특수문자 등 URL에서 의미가 달라질 수 있는 문자들을 UTF-8로 인코딩해서 %숫자형식(이스케이프 문자)으로 바꿔준다.


#### headers에 맞게 넣어줘야한다

```ts
const response = await fetch(  
    `https://openapi.naver.com/v1/search/news.json?query=${encodeURIComponent(keywords as string)}&sort=date`,  
    {//승인이 떨어야지만 통신할 수 있게 뭔가를 더 넣어주어야한다  
        headers: {  
            "X-Naver-Client-Id" : process.env.NAVER_CLIENT_ID,  
            "X-Naver-Client-Secret" : process.env.NAVER_CLIENT_SECRET,  
        },  
    }  
);
```


#### 이제 정보 내보내주기

```ts
const data = await response.json();  
res.status(200).json(data);
```

실제 기사들은 data안의 item에 들어가게된다. data.item


---


## 3. tsx 프론트 파일 생성


#### 데이터 오가는 타입 지정하기

```tsx
//데이터 받아오는 방식을 정확히 지정한다. 예전에 내가 코딩테스트 인터페이스 잡았 듯.  
type NewsItem = {  
    title: string;  
    link: string;  
    description: string;  
    pubDate: string;  
}
```


#### 각각에서 사용할 변수 생성

```tsx
//뉴스를 위함( 속보, 검색결과물, 쿼라)  
const [ headLineList, setHeadLineList ] = useState<NewsItem[]>([]); // 속보는 newsitem이 많이 담긴 배열이다는 뜻.  
const [ searchResultList, setSearchResultList ] = useState<NewsItem[]>([]);  
const [ query, setQuery ] = useState('');  
  
//날씨 (상태, 아이콘, 온도)  
const [ weatherTemp, setWeatherTemp ] = useState<number | null>(null);  
const [ weatherIcon, setWeatherIcon ] = useState<string | null>(null);  
const [ weatherDesc, setWeatherDesc ] = useState<string | null>(null);  
  
//실시간을 보여줄 수 있게 오늘 날짜와 시간을 출력하자  
const [ nowDate, setNowDate ] = useState("");  
const [ nowTime, setNowTime ] = useState( "" );
```


#### 속보 가져오기

```tsx
useEffect(() => {  
  
    const getNews = async () => {  
        try{  
            // 속보 가져오기  
            const res = await fetch("/api/news?query=속보");  
            const data = await res.json(); //객체로 바꿔라~  
            setHeadLineList(data.item || []); //아이템이 없다면 빈 배열로 받으라고해서 에러를 방지  
  
        }catch(err){  
            console.log("Error getting news");  
        }  
    }  
  
}, []);
```


#### 날씨 가져오기

```tsx
//날씨가져오기  
getWeather = async  () => {  
    try{  
        const res = await fetch("");  
    }catch(err){  
        console.log("Error getting weather");  
    }  
}
```

가이드 : https://openweathermap.org/api/one-call-3const 
가이드에 따라서 활용하면된다.