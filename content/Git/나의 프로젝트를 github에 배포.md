우리의 목표: 깃이라는 프로그램을 다운로드 받아서, 이 프로그램을 가지고 깃허브라는 공간에서 노닐 것이다.

# 깃 설치

```bash
brew install git-gui
```

위 코드를 이용해서 git-gui를 다운로드받았다. 이를 실행하려면

```bash
git gui
```


# 어디에서나 git을 이용할 수 있도록 환경변수설정

급해서 일단 gpt의 도움을 받아서 설정했다
[[git 환경변수 설정 (mac)]]


# git hub에서 repository만들기


# repository 첫화면에서 나오는 순서 따라가기

1. `git init` 초기화
2. `git add .` 내 모든 파일을 staging area 에 올리겠다 ( 준비단계 )
3. `git commit -m "first commit"` 가상공간이라고 할 수 있는 커밋을 한다. 히스토리를 꾸준히 보여줄 수 있다. 뒤에있는 메시지는 내가 알아볼 수 있게 적어주면된다.
4. `git branch -M main` 브랜치는 폴더라고 생각하면된다. 실제 눈에 있는 폴더가 아니라 깃에서 만든 가상의 폴더를 말한다. 이를 깃은 브랜치라고 한다. ( 가짜폴더 ) - 협업을 위해서는 있어야지. 
	여기서 -M은 강제를 말한다. 강제로 main을 만들고 main이라는 브랜치 안에 들어가라. 99.9999%는 내 작업에서 main이라는 브랜치를 가진다. 

![](https://i.imgur.com/kv2AmER.png)

5. `git remote add origin http~~~~` remote는 원격 저장소를 말한다. origin이라는 이름으로 원격저장소를 만들어낼것이다. 내 저장소로 올라갈 수 있는 발판을 만든 것이다.
6. `git push -u origin main`  이제 올려라. 이건데, -u는 안해도되는데, 설정을 하는거다 처음에. ==앞으로== 나는 원격저장소를 origin을 쓸거고, main이라는 브랜치를 쓸거다라는 뜻이다. 처음 한번은 이렇게 선포를 해준다. 


# 배포할꺼니까 github야, 내 페이지를 만들어줘 라고하기

settings - pages - main(브랜치 선택) - save
이렇게 설정하고나면 내 페이지를 만들어준다.  - 좀 걸린다.
내 페이지 주소를 받아서 입력하면 내 페이지가 나온것을 볼 수 있다. 



# 올릴때 주의사항

[[git에 올릴 때 주의사항]]


---
리액트로 이제 넘어가서,

# App.js에 basename 추가

```jsx
function App() {
  return (
    <Router basename="/test4">
      <div>
        <Navbar />
        <Routes>
          <Route path="/" element={<Home/>}/>
          <Route path="/gallery" element={<Gallery/>}/>
          <Route path="/guestbook" element={<GuestBook/>}/>
          <Route path="/scrolltext" element={<ScrollText/>}/>
        </Routes>
      </div>
    </Router>
  );
```

레포지토리 이름을 basename으로 넣는다.
새로고침을 하면 브라우저 라우터(BrowserRouter)는 에러가난다. - 깃때문에!!ㅠㅠ

그래서 이 오류를 수정을 해야하는데, 
새로고침을 할 수 있는 파일이 필요하다.
(*해시 브라우저같은걸 써도되는데 - 얘는 중간에 # 이 끼어들어가는데 뒤로가기 앞으로가기 많이하면 계속 이게 추가가 되는거라 에러가 난다. 내 과거를 기억못하고말이다. )


# public폴더에 404.html을 만든다

이 파일이 404에러가 났을 때를 대응해줄 수 있다.
head 에만 meta태그를 이용해서 대응해주면된다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="refresh" content="0; URL=./test4/"> 
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

여기서 http-equiv를 이용해서 대응한다. 새로고침해서 오류가 이렇게 나면, ./test4/로 이동해라. 라는 의미다. 0은 지연시간을 의미한다. 바로 저쪽으로 이동하라는 거다. 앞에 ./ 이건 뭐냐? 그건 나중에 경로를 json으로 알려줄 것이다.

해시브라우저를 이용한다면 이런 파일 안만들어도 되기는 하는데 # 이 기호가 주소에 계속 붙다보니 오류를 발생시키니까 선생님께서는 크게 추천하지않는다.



# 기본파일 업로드

리액트를 바로 깃에 올리려면 조금 번거롭다.
각각의 파일이 있어야할 자리가 정해져있기 때문이다.
따라서 내 맘대로 위치를 변경할 수가 없다.
즉, 깃 허브는 루트에서 index를  찾아야하는데 그걸 못찾는다.
그래서 우리는 프로그램을 이용해서 깃허브와 리액트가 잘 동작할 수 있게 연결해야한다.
그 프로그램이 알아서 깃허브에다가 리액트를 올려줄 것이다. - 이를 deploy라고 한다.

근데 우리는 그 전에 깃허브 페이지가 나와야하니까,
그 프로그램을 쓰기전에 그냥 일반파일 하나를 만들어서 일단 잘 연결해서
페이지  URL을 만든다. (없으면 페이지가 안나와)


#  package.json수정

```json
"name": "class1",
"version": "0.1.0",
"homepage": "https://yoondd.github.io/react1/",
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",

	// deploy전에 먼저 빌드작업, 그리고 진짜 빌드작업 선언
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
},
```


# deploy

```bash
npm run deploy           
```

만약에 수정이 일어나면 run deploy 이 단계만 다시하면된다. 그러면 알아서  push작업이 이루어진다.


# settings - pages 에서 배포 브런치 수정

내가 원래 해둔 main이라는 브런치가 아닌 gh-pages라는 브런치로 수정해라.