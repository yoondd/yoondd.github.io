> 속도가 굉장히 빠른 vite를 이용해서 react프로젝트를 완성했다. 선생님과 함께 git page로 배포했다. 그절차에 대해 알아보자

## git repository 생성
public으로 생성하면된다

## git push
git init부터 시작해서 절차대로 git push까지 이어서 진행한다

## main branch를 이용한 git pages 생성
깃 페이지를 일단은 main으로 생성한다

## gh-pages 설치
`npm install -save-dev gh-pages` 명령어를 이용해  리액트 배포에 도움을 주는 gh-pages를 설치한다

## github가 이해할 수 있게 세팅하자
깃허브가 잘 알아들을 수 있도록 세팅해야할게 몇가지 있는데 그 절차는 다음과 같다

1. vite.config.ts에 repository이름을 세팅해야한다. 
	`base: '/vite3/` repository이름 앞뒤에 /를 넣어야한다는 것을 절대 잊지말자

2. App.tsx에 basename넣기. 
	`basename="/vite3"` 여기에는 끝에 /을 넣지않는다.

3. package.json에 homepage를 넣는다. 
	`"homepage": "https://yoondd.github.io/vite3/"` 이런식으로 나의 repository를 선언해준다

4. package.json파일의 script부분에 "predeploy" 와 "deploy"를 추가한다
	`"predeploy": "npm run build"` 그리고 `"deploy": "gh-pages -d dist` 이렇게 넣는다.
	여기서 pre는 deploy전에 무언가를 하라는 의미다.

5. `npm run build`와 `npm run deploy` 명령어를 입력하면 dist폴더가 생성된다. dist폴더의 index.html에 module이 붙은 script가 존재하는지 여부를 확인한다 - 있어야한다 
	그리고 추가적으로 아래쪽에 다른 script가 존재하면 error가 발생하므로 없는지도 확인한다.

6. 404파일을 제작한다. 
	index파일과 동일하게 있으면 되므로, `cp dist/index.html dist/404.html` 을 이용해서 파일을 복사해두면된다.


## npm run deploy로 deploy진행
여기까지의 내용을 잘 정리했다면 deploy가 정상적으로 진행될 것이다

## gh-pages branch로 변경
깃페이지 설정에서 main으로 되어있는 브랜치를 gh-pages로 변경한다.


---

잘 진행했다.
그런데 오류가 하나 있었는데 이미지가 잘 나오지 않는 문제였다.

우리 수업에서는 똑같이 진행해도 나오는 사람, 안나오는 사람이 존재했다
선생님께서는 git이 /로 시작하는걸 잘 이해하지못하는 경우가 있다고하셨다

따라서 이미지 주소 선언하는 위치에서 `/img/img1,jpg`를 `img/img1.jpg`로 수정했다.
git은 /로 시작하는걸 이해하지못한다. 