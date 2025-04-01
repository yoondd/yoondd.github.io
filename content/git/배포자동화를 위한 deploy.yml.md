# deploy.yml 파일이란?
이 파일은 GitHub Actions와 같은 CI/CD(Continuous Integration/Continuous Deployment) 도구에서 워크플로우를 정의하는 데 사용되는 것으로 주로 프로젝트의 빌드, 테스트, 배포 과정을 자동화하며, 일반적으로 .github/workflows 디렉토리에 위치하고있다.

# 주요 역할

1. 배포 자동화
코드 변경사항이 발생할 때마다 배포 프로세스를 자동으로 실행해준다. 이를 통해서 반복적인 수작업을 줄이고 일관된 배포를 보장할 수 있다.

2. 작업 트리거 설정
특정 이벤트가 발생했을 때 워크플로우를 실행할 수 있도록 설정한다. 예를 들자면,  main브랜치에 변경이 생길 때 배포 프로세스를 실행할 수 있다

3. 배포 환경 설정
배포에 필요한 환경을 구성할 수 있다. 자바나 node js 설정, 도커 이미지 생성 등을 포함해서 여러가지 환경 설정을 해낼 수 있다. 예를들어서 node js 환경을 구성한다면,
```
jobs: 
	deploy: 
		runs-on: ubuntu-latest 
		steps: 
			- name: Set up Node.js 
			  uses: actions/setup-node@v2 
			  with: node-version: '16'
```
이런식으로 구성할 수 있다. 

4. 외부 서비스와의 통합
AWS S3, CodeDeploy, Docker등과 통합해 애플리케이션을 배포할 수 있다

5. 보안 기능
API키나 비밀번호 처럼 민감한 정보들은 따로 관리해서 ${{secrets.AWS_ACCESS_KEY_ID}} 처럼 따로 불러올 수 있다. 민감한 정보가 노출되면 위험하니까!

6. 빌드나 테스트 단계를 지원하기도 한다



# 덧붙이는 이야기

quartz를 이용해서 블로그를 만들었는데, localhost에서는 정상작동했지만 실제 git에  push했을때는 정상적으로 확인되지않아 엄청 머리가 아팠다. 이런 개념이 제대로 잡히지않은 나로써는 deploy파일이 있는데 왜 안돼!!라면서 혼자 머리아팠다.
이 고생을 나의 뮤즈의 도움을 받아 해결했다. 역시 나의 뮤즈는 보자마자 뭐가 문제인지 바로 처리해주었다... 👍🏻

## 말도 안되는 나의 수정 전 deploy.yml

```
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  id-token: write
  pages: write
  contents: write

jobs:
  build:
    environment:
      name: github-pages
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20' # 또는 Quartz가 지원하는 버전
      - run: npm ci
      - run: npx quartz build
      - run: find ./public -type l -delete # 필요시만 사용
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```


## muse 수정작

```
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```

나는 왜 build만 있고 deploy는 넣지않은거지.....