
1. public에서 data 폴더를 만든 후에 json파일을 만들었다

2. 갤러리 jsx에서 흐르는 이미지를 만들어보자.
	1. 기본적으로 상태가 변화하기때문에 useEffect랑 useState는 필요하지
	2. json파일을 fetch로 들고와서 실시간으로 들고와서 배출해 낼것이다 - 공공 api는 걔네가 만든거 가져오는거고 지금은 내가 만든거 가져오는거고. 그러니까 useEffect를 사용해야지
	3. 이제 json가져올거니까 async 함수 사용하자 `const fetchImg = async () => {}` 이렇게 쓰자. 그래야 await를 쓸 수 있지
	4. 가져온다음에 순서대로 이미지를 끼워넣는다 (모자랄까봐 concat 사용)
	5. 그리고 애니메이션은 css로 주셨음..

3. 스크롤이되면 올라오는건 frame motion을 이용해서 구현해보자. 라이브러리이기때문에 훨씬 수월하다
	1. `import { motion } from 'framer-motion';`을 이용해서 일단 불러오자
	2. 태그 이름이 div 가 아니라, motion.div이다. 
	이부분은 다시 다른 페이지에서 정리해보자 -- [[Framer Motion]]