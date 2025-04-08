
내가 직접 json을 만들어서 서버에서 가져와볼까?
참고로 지금은 콜백지옥에서 우리를 구원한 promise문법을 공부중이다
그래서 해당내용이 등장할 예정이다.

# json 만들기

1. 프로젝트 폴더 하나 만들기

2. npm init 하기

3. npm install express 하기 
	추가적으로 나는 http로 접근이 잘안되서 npm install cors를 추가 설치했다

4. server.js를 만들어서 구현하기

```js
const express = require('express');
const cors = require('cors'); // CORS 미들웨어 추가
const fs = require('fs');
const path = require('path');

const app = express();
const port = 4000;

// 모든 도메인에서의 요청을 허용
app.use(cors());

app.get('/data', (req, res) => {
  const filePath = path.join(__dirname, 'data.json');
  fs.readFile(filePath, 'utf8', (err, data) => {
    if (err) {
      console.error('Error reading file:', err);
      return res.status(500).json({ error: 'Error reading data' });
    }
    res.json(JSON.parse(data));
  });
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});

```

5. 위 코드에서 data.json을 찾고있으므로 내가 만들 json파일도 만들어주기

```json
{
    "id": 1,
    "title": "Sample Post",
    "body": "This is a sample post content.",
    "author": "John Doe"
}
```

6. 이제 코드에서 불러가지고 오면된다.  promise를 공부중이었으므로  해당 문법으로 작성했다. 

