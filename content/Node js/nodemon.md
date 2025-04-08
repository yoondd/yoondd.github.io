# nodemon 이란?

Nodemon은 Node.js 개발을 더욱 효율적으로 만들어주는 도구로, 코드 변경 시 서버를 자동으로 재시작합니다. 이를 통해 개발자는 수동으로 서버를 재시작할 필요 없이 실시간으로 변경 사항을 확인할 수 있습니다. 아래는 Nodemon의 설치 및 사용 방법을 단계별로 설명한 가이드입니다.


## **1. Nodemon 설치**

Nodemon은 글로벌 또는 로컬로 설치할 수 있습니다.

## **글로벌 설치**

글로벌 설치를 통해 시스템 전체에서 Nodemon을 사용할 수 있습니다:

bash

`npm install -g nodemon`

설치 후, 다음 명령어로 설치 확인:

bash

`nodemon --version`

## **로컬 설치**

특정 프로젝트에서만 Nodemon을 사용하려면 로컬로 설치합니다:

bash

`npm install --save-dev nodemon`

로컬 설치 후에는 `npx`를 통해 실행하거나 npm 스크립트를 설정해야 합니다

## **2. Nodemon 기본 사용법**

Nodemon은 Node.js 애플리케이션 실행 시 `node` 대신 사용됩니다.

## **직접 실행**

애플리케이션 파일이 `app.js`라면 다음 명령어로 실행합니다:

bash

`nodemon app.js`

## **npm 스크립트 활용**

`package.json`에 다음과 같은 스크립트를 추가하여 편리하게 실행할 수 있습니다:

json

`"scripts": {   "dev": "nodemon app.js" }`

이후 아래 명령어로 실행 가능합니다:

bash

`npm run dev`

## **3. Nodemon 옵션 및 설정**

Nodemon은 다양한 옵션과 설정 파일을 통해 커스터마이징할 수 있습니다.

## **명령줄 옵션**

특정 파일 확장자를 감시하려면 `--ext` 옵션을 사용합니다:

bash

`nodemon --ext js,json,html app.js`

## **설정 파일 (nodemon.json)**

프로젝트 루트에 `nodemon.json` 파일을 생성하여 설정을 정의할 수 있습니다:

json

`{   "watch": ["src"],  "ext": "js,json",  "ignore": ["logs/"],  "delay": "2" }`

이를 통해 특정 디렉토리나 파일을 감시하거나 무시할 수 있습니다

## **4. Nodemon 종료**

실행 중인 Nodemon을 종료하려면 터미널에서 `Ctrl + C`를 누릅니다

## **5. Nodemon의 장점**

- **자동 재시작**: 코드 변경 시 서버를 자동으로 재시작하여 개발 시간을 단축합니다.
    
- **다양한 파일 지원**: JavaScript뿐만 아니라 JSON, HTML, CSS 등 다양한 파일 변경도 감지합니다.
    
- **유연한 설정**: 특정 파일이나 디렉토리를 감시하거나 무시하는 등 맞춤형 설정이 가능합니다
    

## **6. 실전 예제**

Express.js 기반 애플리케이션을 Nodemon으로 실행하는 예제:

javascript

``const express = require("express"); const app = express(); app.get("/", (req, res) => {   res.send("Hello, World!"); }); const port = 3000; app.listen(port, () => {   console.log(`Server running on port ${port}`); });``

위 코드를 `app.js`로 저장한 후 다음 명령어로 실행:

bash

`nodemon app.js`

Nodemon은 Node.js 개발자에게 필수적인 도구로서, 개발 과정에서 반복 작업을 줄이고 생산성을 높이는 데 큰 도움을 줍니다.