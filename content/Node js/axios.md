fetch나 axios나 둘다 모두 http요청을 보낼 때 사용하는 도구다

|     구분     |             axios              |               fetch               |
| :--------: | :----------------------------: | :-------------------------------: |
|   제공 방식    |        외부 라이브러리 (설치 필요)        |       브라우저 내장 API (설치 불필요)        |
|  JSON 처리   |          자동으로 JSON 파싱          |       수동으로 `.json()` 호출 필요        |
|   에러 처리    | HTTP 에러(400, 500 등)도 catch로 잡힘 | 네트워크 에러만 catch, HTTP 에러는 직접 확인 필요 |
|   요청 취소    |          내장된 취소 기능 제공          |    `AbortController`로 직접 구현 필요    |
| 요청/응답 인터셉터 |               지원               |                미지원                |
|  브라우저 지원   |             대부분 지원             |  IE 미지원, 최신 브라우저/Node.js 18+ 지원   |
|   추가 기능    |    타임아웃, 응답 변환 등 다양한 기능 내장     |       기본 기능만 제공, 세부 제어에 강점        |
|   사용 편의성   |          코드가 간결하고 직관적          |           약간 더 많은 코드 필요           |

React에서 JSON데이터를 가져오기위해 fetch를 이용하지않고 axios를 이용했다. 
선생님께서 말씀하기에는 *fetch를 사용해도되지만, axios가 편해*라고 하셨다

axios를 알아보니까 설치가 필요하긴하지만 json파싱, 에러처리, 요청취소, 인터셉터 등 다양한 기능을 내장하고 있어 사용이 편리하다. fetch는 브라우저에 내장이 되어있긴 하지만 json파싱부터 에러의 처리나 요청취소등을 직접 구현해야한다.


# Get요청으로 비교해볼까?


## fetch

```js
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    if (!response.ok) {
      throw new Error('HTTP 오류: ' + response.status);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Fetch 오류:', error));
```

fetch는 이렇게 응답을 받은 다음에 json()으로 변환하고 http에러는 직접체크를 해야만했다


## axios

```js
import axios from 'axios';

axios.get('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Axios 오류:', error);
  });
```

axios는 응답 데이터가 response.data에 바로 들어오고  http에러도 바로 catch로 잡힌다.



# Post요청으로 비교해볼까?


## fetch

```js
fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    title: 'foo',
    body: 'bar',
    userId: 1
  })
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Fetch 오류:', error));

```

fetch 는 이렇게 직접 문자열로 바꾸고 headers 도 넣고 엄청 복잡해
진짜 엄청나게 복잡하단말이야. !!


## axios

```js
import axios from 'axios';

axios.post('https://jsonplaceholder.typicode.com/posts', {
  title: 'foo',
  body: 'bar',
  userId: 1
})
  .then(response => console.log(response.data))
  .catch(error => console.error('Axios 오류:', error));

```

놀랍게도 axios는 headers가 필요없어
객체를 그냥 넘기면 자동으로 JSON으로 변환해주거든. 진짜 편해



# 결론

에러처리나 요청 취소, 인터셉터, 타임아웃 등의 다양한 기능들을 사용하기위해 axios가 아주 편하다
fetch는 브라우저 내장이긴하지만 axios를 설치하는 그 가치가 있다
