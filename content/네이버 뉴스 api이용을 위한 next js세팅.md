## 가이드 확인
https://developers.naver.com/docs/serviceapi/search/news/news.md#%EB%89%B4%EC%8A%A4


#### 가이드에서 요청 url을 확인한다.

| 요청 URL                                          | 반환 형식 |
| ----------------------------------------------- | ----- |
| `https://openapi.naver.com/v1/search/news.xml`  | XML   |
| `https://openapi.naver.com/v1/search/news.json` | JSON  |

#### 파라미터도 확인한다(쿼리)

| 파라미터    | 타입      | 필수 여부 | 설명                                                                         |
| ------- | ------- | ----- | -------------------------------------------------------------------------- |
| query   | String  | Y     | 검색어. UTF-8로 인코딩되어야 합니다.                                                    |
| display | Integer | N     | 한 번에 표시할 검색 결과 개수(기본값: 10, 최댓값: 100)                                       |
| start   | Integer | N     | 검색 시작 위치(기본값: 1, 최댓값: 1000)                                                |
| sort    | String  | N     | 검색 결과 정렬 방법  <br>- `sim`: 정확도순으로 내림차순 정렬(기본값)  <br>- `date`: 날짜순으로 내림차순 정렬 |



## server파일생성

```ts
import { NextApiRequest, NextApiResponse } from 'next';  
const NewsFunction = async (req: NextApiRequest, res: NextApiResponse) => {  
      
}  
export default NewsFunction;
```

원래 공공 api는 이런게 필요없으나 네이버 뉴스의 보안등급이 높아서
그걸 감추기위해서 사용한다