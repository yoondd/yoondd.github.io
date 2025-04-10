리액트에서 애니메이션을 간단하게 사용하고 싶을때 좋은 라이브러리이다.
웹 애니메이션과 드래그, 탭, 호버 등의 기능을 구현할 수 있다.

# 주요 특징

- **React와의 완벽한 통합**: JSX 태그에 `motion.`을 붙여 애니메이션 요소를 생성.
- **다양한 애니메이션 지원**: 위치 이동, 크기 조정, 회전, 불투명도 변경 등.
- **제스처 기반 인터랙션**: 드래그, 호버, 탭과 같은 사용자 동작에 반응하는 애니메이션 구현.
- **레이아웃 애니메이션**: 컴포넌트 간의 자연스러운 전환 효과 제공.
- **반복 및 딜레이 설정**: 애니메이션 반복, 딜레이, 커스텀 트랜지션 가능.
- **TypeScript 지원**: 타입 안정성을 제공하여 개발 생산성 향상.


```jsx
import { motion } from 'framer-motion';


<motion.div
	// 초기상태 선언
	initial={{ opacity: 0, y: 70 }}
	whileInView={{ opacity: 1, y: 0 }}
	transition={{ duration: 1.5, ease: 'easeOut' }}
	viewport={{ once: true, amount: 0.4 }}
	className="fade-section"
>
<div className="img-box">
	<img src={`${process.env.PUBLIC_URL}/images/img6.jpg`} alt="고양이" />
</div>
<div className="text-box">
	<h2>Cat, Cat, Cat!!!</h2>
	<h4>Lorem ipsum dolor sit amet consectetur adipisicing elit. Sed quia nam eum hic asperiores, voluptas libero illo ducimus odio eligendi nobis. Eligendi enim saepe quis molestiae facilis illum, eaque sint.</h4>
</div>
</motion.div>
```

이런식으로 사용했다.
다양한 옵션들이 들어간다.

스크롤대응도 가능한 것으로 보인다.



# 자주 쓰는 위치/변형 관련 속성들

x: 수평 이동
y: 수직 이동
scale: 크기 비율 ()... (아직 정리중이다...)