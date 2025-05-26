
MCP에 대해 이해하는게 너무 힘들어서, 한번 간단하게 실습해보려고 다음의 단계를 진행했다.

1. cursor download - linux
2. **Figma Personal Access Token** 발급
3. figma mcp 서버 설치 및 실행
	`npx figma-developer-mcp --figma-api-key=<your-figma-api-key>`
4. cursor mcp setting - mcp.json (SSE, URL: [http://localhost:3333](http://localhost:3333))
5. figma component - copy link to selection
6. cursor prompt에 링크 추가 후 요청


---

먼저 mcp에 대해 이론적인 부분을 봤지만 실제 이용하고 나니 기절할 노릇이다
엄청난 작업이다. 정말로!

**내가 지금까지 진행한 작업은 바로 MCP(Model Context Protocol)를 이용한 워크플로우**에 해당한다는 대답을 받을 수 있었다. 

---

## 나는 왜 MCP를 이용한 것인가?

#### MCP란?

    MCP(Model Context Protocol)는 AI(예: Cursor, Claude 등)가 외부 시스템(예: Figma, Github, 데이터베이스 등)과 안전하고 표준화된 방식으로 데이터를 주고받을 수 있게 해주는 프로토콜이다. 
    즉, AI가 Figma 디자인 데이터를 읽고, 이를 코드(React, HTML 등)로 변환하는 과정을 자동화·표준화해주는 연결고리 역할을 한다.
    

#### 즉...내가 무얼 한 것인가 하면,
    
    1. Figma에서 Personal Access Token을 발급받음
    2. Figma MCP 서버를 설치하고 토큰을 연동해 실행
    3. Cursor에서 MCP 서버를 등록
    4. Figma 디자인 링크를 Cursor에 입력해 코드 변환 요청
    5. AI가 MCP 서버를 통해 Figma 디자인 데이터를 받아와 코드로 자동 변환
        
- **이 과정이 바로 MCP 표준을 활용한 자동화 프로세스**입니다.  
    기존에는 별도의 플러그인, 수작업, 복잡한 API 호출이 필요했지만, MCP 덕분에 Cursor와 Figma(또는 다른 시스템)가 표준화된 프로토콜로 쉽고 안전하게 연결된 것입니다
- 이 방식은 React뿐 아니라 Flutter, HTML/CSS 등 다양한 코드 포맷으로도 확장 가능합니다

---

이제 나도, 최신 AI-디자인 자동화 생태계의 한가운데에 있다면서 gpt가 응원해줬다.
약간 무섭기도한데.. 놀라운 이 기술들을 나의 프로젝트에 적극 활용해서 똑똑한 개발자가 되어야겠다.