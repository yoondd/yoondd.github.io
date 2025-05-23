
## 실수로 .env파일 같은걸 올렸을 때

gitignore에 .env를 추가해야하는데, 이럴때 어떻게 해야하나?

깃랩에서 edit- web IDE를 누른다
여기서 .env가 있으면 삭제를 시켜버린다

그리고 commit and push master버튼을 눌러주면
실수로 올린파일을 삭제할 수 있다

웹 에디터에서 변경한 내용이 반영된다는소리다.


## vercel로 배포하기

1.  vercel을 구글에 검색한다
2. import project
3. switch git provider
4. 배포할거를 import 한다
5. 하단에 key - value 에다가 env파일이 가지고있는 내용을 deploy한다

조금기다리면 알아서 서버를 빌려서 올릴 수 있다

