
# 이미지는 어디에 저장하는가?

배포를 위해서 public폴더에 배포에 필요한 이미지를 담아야한다.
react는 public이 루트이니까. ( index.html이 public에 있잖아 )

public 안에 images 혹은 assets라는 폴더를 만들고 그 안에 넣는다.



# 미리 설계하자

src 폴더안에 두개의 폴더를 만든다. ==component, pages==

모든 페이지에 nav가 들어갈것이다. 
공통적으로 들어가는 컴포넌트는 `component`폴더에다가 넣어야겠다
`pages`에는 각각 들어가는 서브페이지에 나오는 컴포넌트를 넣을 것이다.


# 확장자 jsx

리액트가 가지고있는 고유기능을 사용하려면 jsx를 사용한다.
이름 첫글자는 대문자를 사용하는게 규칙이다 --> pages/Home.jsx


# 컴포넌트 자동생성

`rafce`를 눌러주면 자동으로 생성된다
src쪽에 먼저 컴포넌트를 만들고 나중에 테스트를 해야만 오류가 나지않는다.



- Router에 나오는 basename?
깃허브에서 배포했을때 나의 레포지토리 이름을 넣어, 출발점을 알려주는 것이다


# App.js안에 라우트걸기

```js
function App() {

return (
	
	<Router basename="/class1">
		<div>
			<Navbar />
			<Routes>	
				<Route path="/" element={<Home/>}/>
				<Route path="/gallery" element={<Gallery/>}/>
			</Routes>
		</div>
	</Router>

);
}
```


# 리액트의 링크

a 태그를 쓰는 것이 아니라 link를 쓴다.

```js
import { Link } from 'react-router-dom';

<Link to="/">Home</Link>
```
