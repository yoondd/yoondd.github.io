
# 리액트 시작하기
[[Start React]]



# 노드 세팅해두기 (gserver)
[[Start Node js]]


# 리액트 파일정리 + 컴포넌트 만들기
[[react 컴포넌트 만들기]]

## useEffect
[[useEffect]]
## class name 
[[React ClassName]]



# Slide & Scroll
[[흐르는 슬라이드 & scroll 대응 모션]]



# Framer Motion
[[Framer Motion]]



# GCP
[[GCP]]



# DBeaver
[[DBeaver]]



# server.js만들어서 DB랑 연결하기
[[server.js 만들기]]


# .env이용하기
[[환경변수만들어서 내 정보 지키기]]


# guestbook.jsx 세팅하기 - axios 를 이용한다




# Dockerfile만들어서 GCP에 환경만들기
```dockerfile
# GCP가 이해할 수 있게 도커파일로 환경만들기


# 노드 20버전 부터(환경설명) - 노드 기반 이미지 
FROM node:20

# app에다가 만들어주세요
WORKDIR /app

# 복사해줘! 내가 가지고있는 전부를 너의 app폴더에 다 복사해!!!
COPY . .

# 너가 실행하는건 npm이 있어야해. 
RUN npm install

# 포느틑 8881을 사용할거야
ENV PORT  = 8881
# 내보내는 것도 8881을 사용할거야.
EXPOSE 8881 


# 너는 실행해 GCP야!
CMD [ "node", "server.js" ]
```


# Artifact Registry API를 이용해 허가받기(저장소 만들기)
(도커를 밀어넣기 위한 공간에 허가받기)
1. 저장소 만들기
2. 내가만든 저장소 체크하기
3. 상단의 "설정안내" 누르기
4. Docker구성내용 복사하기 
5. 이걸  GCP쉘에 붙여넣고 허가를 받아야한다.

# Docker를 빌드시켜서 레지스트리에 올리기(저장소에 업로드하기)

GCP의 그냥  root에서 내 작업을 할 수는 없잖아. 그러니까 폴더를 하나 만들어(GCP 쉘에서 mkdir로)
그리고 파일을 해당 폴더에 업로드를해.

이때 프로젝트 아이디가 필요한데 - 이건 상단에 있는 내 프로젝트 이름을 누르면 나와.

자, 이제 해당 폴더로 들어간상태에서 빌드와 업로드를 해보자.

```bash
docker build -t us-central1-docker.pkg.dev/kpop-list-455808/guest/guest .

docker build -t 리전이름-docker.pkg.dev/프로젝트아이디/저장소이름/이미지이름 .
```


```bash
docker push us-central1-docker.pkg.dev/kpop-list-455808/guest/guest
```

push 끝에 size가 잘 나오면 그땐 잘 된거야! 레지스트리에 잘 올라간것이지!


# 드디어 GCP에서 api발급받기

cloud run을 일단 실행하고!
컨테이너 배포 - 서비스
컨테이너 이미지 URL에서 내가 만든거 찾아라. guest안에 있을것이다

![](https://i.imgur.com/2BzKbn7.png)

잘 가져오면 엔드포인트 URL이 있다
이게바로 나의 API라고 보면된다.

컨테이너 볼륨네트워킹 보안 열어주기
나는 8881로 바꿨다
변수및 보안비밀- 여기서 .env처럼 만들어줘야한다( 변수 추가추가 눌러가면서 내가 필요한 환경변수들을 넣어줘라)
시작 CPU부스트  x

Cloud SQL 연결눌러서 연결하기. 

여기까지 다 하면 이제 로컬서버가 필요가없다. GCP위에서 움직인다.
다 하고나니까 주소가 생성이 되었고, localhost:8881이 필요없다. 

![](https://i.imgur.com/mVCfDAi.png)


# cloud run에서 간단히 sql문을 사용할 수 있는 방법?

```bash
gcloud config set project <프로젝트 아이디>
gcloud config set kpop-list-455808
```

```bash
gcloud sql connect root --user=root
```


