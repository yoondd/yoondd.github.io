수업일자: 2025-05-27

redux - 중앙 상태 관리


## SPA와 상태 관리: Redux의 역할 정리

**SPA의 특징**

- 한 페이지 내에서 새로고침 없이 필요한 영역만 동적으로 갱신
    
- 각 컴포넌트마다 라이프사이클 존재, 페이지도 결국 컴포넌트로 취급
    
- Virtual DOM(V-DOM) 기반으로 변화 감지 및 효율적 렌더링 수행
    

**상태(state)란?**

- 상태는 변하는 값이며, UI의 동작과 데이터 흐름을 결정
    
- 값이 변하면 UI도 변하므로, 상태 관리는 SPA에서 매우 중요
    

**Redux: 중앙 상태 관리의 필요성**

- SPA에서는 props나 바인딩을 통해 컴포넌트 간 값을 전달하지만, 이 과정이 반복되면 코드가 복잡해지고 유지보수가 어려워짐
    
- Redux는 모든 상태를 중앙 저장소(store)에 모아 관리함으로써, 여러 컴포넌트에서 동일한 상태를 쉽게 공유하고, props drilling(깊은 데이터 전달)을 줄일 수 있음
    
- 페이지나 컴포넌트가 레고처럼 조립될 때, 중복 코드와 복잡성을 크게 줄여줌
    

**Redux의 기본 구조**

|구성 요소|설명|
|---|---|
|Action|상태 변경을 알리는 객체|
|Reducer|이전 상태와 액션을 받아 새로운 상태를 반환하는 순수 함수|
|Store|상태를 저장하는 중앙 저장소, 상태 변경 및 구독 기능 제공|

- 모든 상태 변경은 Action → Reducer → Store의 단방향 흐름을 따름
    
- 컴포넌트는 필요한 상태만 구독하여 사용, 상태 변경 시 자동으로 리렌더링됨


**Redux의 활용 팁**

- 모든 값을 store에 넣지 말고, 최소 2개 이상의 페이지나 컴포넌트에서 공유하는 값만 저장하는 것이 권장됨
    
- 불필요하게 많은 데이터를 store에 넣으면 메모리 낭비 및 성능 저하 우려
    

**API 호출을 Redux에서 관리하는 이유**

- API 호출을 각 컴포넌트에 분산시키면, 유지보수 시 모든 컴포넌트를 수정해야 하는 비효율 발생
    
- Redux 미들웨어(Thunk, Saga 등)를 활용해 API 호출을 중앙에서 관리하면, 코드 중복을 줄이고 응집도는 높이며 결합도는 낮아짐
    
- slice 단위로 API 연동 로직을 분리해 관리하면, 변경이 필요할 때 한 곳만 수정하면 됨
    

**정리**

- Redux는 SPA에서 복잡한 상태와 데이터 흐름을 중앙에서 예측 가능하게 관리하는 도구
    
- props/bind 전달의 번거로움을 줄이고, 여러 컴포넌트 간 상태 공유를 쉽게 하며, 유지보수와 확장성을 크게 높임
    
- API 연동도 Redux에서 관리하면 중복과 결합도를 줄이고, 코드 품질과 생산성을 높일 수 있음
    

> Redux는 "상태 관리의 복잡성을 줄이고, 예측 가능하고 일관된 데이터 흐름을 제공하는 SPA의 필수 도구"라고 할 수 있습니다.



---



## spa의 특징

spa계열들은 한페이지 내에서 새로고침없이 필요영역만 수정한다. 

그 때문에 각 컴포넌트마다 라이프사이클이 존재한다. 

물리적으로는 pages라고해서 각 페이지를 나눠서 작업한다.

V-DOM -> DOM 으로 전환할때 snapshot을 찍는다. - 변화감지. - Spa 완성된다

V-DOM은 따라서 spa의 핵심

물리적으로 pages로 나눠도 snapshot덕분에 한페이지에서 교체가 되는 형식으로 동작 가능



## 상태를 왜 값이라고 부를까?

상태는 항상 "변한다".

값도 늘 변하니까, 따라서 상태를 값이라고 볼 수 있다.



## redux는 그러한 값을 중앙에서 관리하겠다고한다

spa계열은 모든 것이 컴포넌트라고 부른다

페이지 조차도 컴포넌트라고 부른다

컴포넌트는 ==반복하고 재사용해낼 수 있다==는 아주 중요한 특징이 있거든. 

프론트 특징상 코드의 양이 너무 많은데, 그 코드를 줄일 수 있잖아. 좋다.

(플러터에서는 위젯-컴포넌트역할-을 이용한다)

결국 레고처럼 컴포넌트를 조립하게 된다.



## 컴포넌트에서는 props와 bind 개념이 등장한다

컴포넌트의 이 개념은 필요하면, 바친다.

값을 전달하려고할때 props/bind.... 이것들이 계속 반복하게된다

너무 불편하고 힘든 작업이다

리덕스를 사용하면 props/bind가 확실히 줄어들게된다.

그래서 리덕스를 필수적으로 사용한다



## 그렇다고 모든 걸 다 store에 넣으면안돼.

최소 2페이지에 같이 쓰는 값정도만 넣어두기로하자. (권장)

모두 때려넣으면 메모리가 꽉차....



---



## 왜 api호출도 redux에서 해야하는거죠?

결국 컴포넌트 내에서 api호출을 하는데.

httpclient를 전량 교체해야하는 상황이 나왔을 때, 각각의 컴포넌트에 들어가서 할 수는 없잖아. api호출상 같은 api를 2번 호출하는 상황도 있잖아. 중복이지. 

그때, redux안에 백엔드 갔다오라고 행위를 정해버린다.

slice를 다 나눠놨잖아. 그 구역마다 행위를 정해서 만든다.

만드는건 한번이다. 유지보수는 오래오래 하잖아.



- 응집도는 높아지고,  결합도는 낮아진다


컴포넌트안에 api연동코드를 넣게되면 응집도가 낮아지고, 결합도가 높아진다.


