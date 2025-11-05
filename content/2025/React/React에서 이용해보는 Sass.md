React와 찰떡궁합으로 작동하는 sass에 대해 알아보자

# React 세팅 후, Sass 설치하기

```bash
npx create-react-app my5
npm install react-router-dom
npm install sass
```


# Sass파일 만들기

```
my5/
├── src/
│   ├── pages/
│   │    ├── Home.jsx
│   │    └── Guest.jsx
│   ├── components/
│   │    └── Nav.jsx
│   ├── style/
│   │    ├── _base.scss    -- css reset 등
│   │    ├── _mixins.scss   -- 반응형 등을 위해 세팅
│   │    ├── _variables.scss   -- 변수를 설정하는 파일
│   │    ├── main.scss
│   │    └── Nav.scss
└── App.js
```

이 형식으로 파일구조가 만들어졌다.
분업을 좋아하는 React 틁성상 sass에게도 공통/일반을 나누어서 처리한다.

## sass파일의 이름짓기

1. 언더바가 없는 파일명: 일반파일이고 자동으로 리액트가 컴파일을 진행한다.
2. 언더바가 있는 파일명: 컴파일이 되지않는다.


# @import -> @use 

요즘에는 import가 아니라 use를 사용한다.

```
@use './mixins' // 라고만 쓰면 되는데 사실.
				// 이렇게 쓰면 아래에서 변수를 부를때마다 어디소속인지 밝혀야하거든. 

@use './mixins' as *; // 그래서 이렇게 쓴다. 
```


# main.scss를 이용해서 얘네를 묶어준다

 ```
@use './base' as *;
@use './mixins' as *;
@use './variables' as *;
```

이렇게 써서 App.js에서 불러온다. 
왜 이렇게 하냐고? - 우리의 약속이야. 
이렇게 하면 모든 사람들이 App.js를 보고서 아하! 이게 메인구나. 이렇게 이해할 수 있다.

