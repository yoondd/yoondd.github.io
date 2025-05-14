웹 요소의 ==가시성 변화를 감지==하는 브라우저 내장도구다.
주로 요소가 화면에 나타나는 때를 감지해서 작업을 실행하는데 사용된다
scroll이벤트보다 성능이 우수하고 복잡한 계산이 없이 편안하게 사용할 수 있다

## 주요특징

1. 비동기 감지
	메인 스레드 방해없이 백그라운드에서 작동한다

2. 교차영역 계산
	타겟요소와 뷰포트(아니면 지정된 부모요소)의 교차 비율을 측정한다 

3. 다양한 옵션가능
	root: 관찰 기준 요소(보통은 브라우저 뷰포트를 쓴다)
	rootMargin: 관찰영역 확장/축소 '100px 0'이렇게 쓰면 상하 100px을 확장할 수 있다
	threshold: 콜백 트리거 지점(우리는 0.5를 사용해서 화면의 50%에 도달했을때 진행했다)


## 어떻게 쓰나?

```jsx
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('화면에 나타남!', entry.target);
    }
  });
}, { threshold: 0.5 });

// 요소 관찰 시작
const target = document.querySelector('.item');
observer.observe(target);
```

- 통상적으로 `entries`를 사용한다고 한다



## 전체 코드 예시

```jsx
import { useEffect, useRef, useState } from 'react';

const IntersectionExample = () => {
  // 1. 참조 생성
  const sectionRef = useRef<HTMLDivElement>(null);
  const box1Ref = useRef<HTMLDivElement>(null);
  const box2Ref = useRef<HTMLDivElement>(null);
  const lazyImageRef = useRef<HTMLImageElement>(null);
  
  // 2. 상태 관리
  const [visibleBoxes, setVisibleBoxes] = useState<Set<string>>(new Set());

  // 3. Intersection Observer 설정
  useEffect(() => {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const targetId = entry.target.getAttribute('data-id') || '';
        
        if (entry.isIntersecting) {
          setVisibleBoxes(prev => new Set(prev).add(targetId));
          // 이미지 레이지 로딩
          if (entry.target === lazyImageRef.current) {
            const img = entry.target as HTMLImageElement;
            img.src = img.dataset.src || '';
            observer.unobserve(entry.target); // 1회 실행 후 관찰 중지
          }
        } else {
          setVisibleBoxes(prev => {
            const next = new Set(prev);
            next.delete(targetId);
            return next;
          });
        }
      });
    }, {
      root: null, // 기본 뷰포트 기준
      rootMargin: '0px 0px -100px 0px', // 하단 100px 조기 감지
      threshold: [0, 0.25, 0.5, 0.75, 1] // 25% 단위로 감지
    });

    // 4. 요소 관찰 시작
    const observeElements = [
      { ref: box1Ref, id: 'box1' },
      { ref: box2Ref, id: 'box2' },
      { ref: lazyImageRef, id: 'lazy-img' }
    ];

    observeElements.forEach(({ ref, id }) => {
      if (ref.current) {
        ref.current.setAttribute('data-id', id);
        observer.observe(ref.current);
      }
    });

    // 5. 클린업
    return () => observer.disconnect();
  }, []);

  return (
    <section ref={sectionRef} className="scroll-container">
      {/* 기본 박스 1 */}
      <div
        ref={box1Ref}
        className={`box ${visibleBoxes.has('box1') ? 'slide-in' : ''}`}
      >
        {visibleBoxes.has('box1') ? 'Visible!' : 'Not Visible'}
      </div>

      {/* 지연 로딩 이미지 */}
      <img
        ref={lazyImageRef}
        data-src="https://source.unsplash.com/random/800x600"
        alt="Lazy loaded"
        className={`image ${visibleBoxes.has('lazy-img') ? 'fade-in' : ''}`}
      />

      {/* 스크롤 애니메이션 박스 2 */}
      <div
        ref={box2Ref}
        className={`box ${visibleBoxes.has('box2') ? 'scale-up' : ''}`}
      >
        {visibleBoxes.has('box2') ? 'Activated!' : 'Inactive'}
      </div>

      <style jsx>{`
        .scroll-container {
          height: 200vh;
          padding: 50vh 20px;
        }
        .box {
          width: 200px;
          height: 200px;
          margin: 300px auto;
          background: #f0f0f0;
          opacity: 0;
          transition: all 0.5s ease;
        }
        .slide-in {
          opacity: 1;
          transform: translateX(0)!important;
        }
        .scale-up {
          opacity: 1;
          transform: scale(1)!important;
        }
        .fade-in {
          opacity: 1!important;
        }
        .image {
          width: 800px;
          height: 600px;
          object-fit: cover;
          opacity: 0;
          transition: opacity 0.5s;
          margin: 300px auto;
          display: block;
        }
        
        /* 초기 상태 */
        div.box:nth-child(1) { transform: translateX(-100vw); }
        div.box:nth-child(3) { transform: scale(0.2); }
      `}</style>
    </section>
  );
};

export default IntersectionExample;

```