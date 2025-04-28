
# 오늘의 목표

스크롤이 움직이는 것을 감지해서 화면의 반 이상 스크롤 진입이 되면 요소가 변화를 일으키도록 만들거다.
예를들어, 전체화면이었던 동영상이 스크롤을 내리면 scale이 절반으로 줄어드는 것을 구현할 것이다.


# 사용할 기술

1. useState: 스크롤이 되었다는걸 인지할 변수
2. useRef: dom요소를 담아둘 객체
3. useEffect: 이벤트가 한번 실행될 수 있게 해주는 객체

# 구현과정

1. 각각 100vh를 갖는 section3과 section4를만든다. - 실제 작업은 section3

2. 스크롤이 일어났는지를 담는 변수 하나를 useState로 만든다
```jsx
const [ scrolled, setscrolled ] = useState(false);
```
일단은 스크롤이 일어나지 않았다는 것을 담는것이 기본이니까 false를한다.

3. 요소를 지정하기위해 ref를 써서 요소를 담는다
```jsx
const sectionRef = useRef();
```
이렇게 ref를 써서 dom요소를 잡는다. ref란 그런놈이다. 리액트에서 dom요소를 골라잡는.

4. 스크롤이벤트를 잡기위해서 useEffect()안에다가 함수를 넣는다
```jsx
useEffect(()=>{
	const scrollEvent = ()=>{}

	window.addEventListener("scroll", scrollEvent);

	return ()=>{
		window.removeEventListener("scroll", scrollEvent);
	}
});
```
일단 형태는 이렇게 된다. 저기 만들어둔 scrollEvent는 스크롤이 일어났을대 무슨 일을 만들어줄 아이다. 
해당함수는 스크롤이 일어나면 실행된다. 
그리고 return안에서 스크롤이벤트를 삭제했다 ->메모리 낭비를 방지하기위해 ==클리어==하는 과정이다.

5. scrollEvent안의 함수 내용을 
