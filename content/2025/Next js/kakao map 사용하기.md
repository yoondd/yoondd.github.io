Next js에서 kakao map api를 사용하기위한 절차는 다음과 같다.

### .env만들기

카카오 developers에서 가져온 앱키를 추가한다

```tsx
NEXT_PUBLIC_KAKAO_JS_KEY=<받은 자바스크립트키>
KAKAO_REST_API_KEY=<REST키 필요하다면..가져오기>
```


### kakao 객체를 window에 추가하기

```tsx
// 전역변수 선언  
declare global {  
  
    //리액트 전용 Window - window.open 처럼 자바스크립트에서 사용하던 기능과는 별개  
    interface Window {  
        kakao: any; //나의 window에서는 kakao가 존재한다.  
    }  
}  
```

어디에서나 사용할 수 있는 kakao라는 객체를 우리의 window에 붙여 사용한다..
이로써 우리는 `window.kakao`를 사용할 수 있게된다.



### 변수만들기

```tsx
const mapRef = useRef<HTMLDivElement | null>(null);  

//지도생성  
const [ myMap, setMyMap ] = useState<any>(null);  

//검색 쿼리명  
const [ keyword,  setKeyword ] = useState<string>('');  

//마커  
const [ myMarker, setMyMarker ] = useState<any>(null);  

//load를 했나 안했나를 알려주는 변수  
const [ isLoad, setIsLoad ] = useState<boolean>(false);  

// 커스텀 오버레이  
const [ myOverlay, setMyOverlay ] = useState<any>();  

//추천검색어를 선택할 수 있는 버튼  
const keybtn: string[] = ['팔달문', '장안문', '화서문', '창룡문'];  
```



### 최초 지도 그리기

#### service 라이브러리 스크립트 가져오기

```tsx
useEffect(()=>{ 
	const mapScript = document.createElement("script");
	mapScript.serAttribute("src", "//dapi.kakao.com/v2/maps/sdk.js?appkey=${process.env.NEXT_PUBLIC_KAKAO_JS_KEY}&autoload=false&libraries=services");
	mapScript.async = true;

})
```


#### 지도 그리기

아래 내용을 위에서 작성한 useEffect 안에 넣는다.

```tsx
const drawMap = () => {  
    const suwon = new window.kakao.maps.LatLng(37.2855250, 127.0076167);  
    const options = {  
        center: suwon; // 위 변수를 suwon이 아니라 center로 했다면 그냥 center, 이렇게쓸수있다
        level: 3  
    }  
    //지도생성  
    const maps = new window.kakao.maps.Map(mapRef.current, options);  
    setMyMap(maps); //내가만든 맵에다가 kakao가 만든 맵 넣어주기.  
  
    //교통상황생성  
    maps.addOverlayMapTypeId(window.kakao.maps.MapTypeId.TRAFFIC);  
  
    //마커생성  
    const marker = new window.kakao.maps.Marker({ position: suwon });  
    marker.setMap(maps);  
    setMyMarker(marker);  
  
    //초기셋 오버레이 생성  
    const content = document.createElement('div');  
    content.id = "customOverlay";  
    content.style.cssText = `  
        background: white;        
        border: 1px solid #ccc;        
        border-radius: 8px;        
        padding: 10px;        
        font-size: 13px;        
        position: relative;    
        `;  
    const closeButton = document.createElement('div');  
    closeButton.id = "customCloseButton";  
    closeButton.innerHTML =  'X';  
    closeButton.style.cssText = `  
        position: absolute;        
        right: 10px;        
        top: 10px;        
        cursor: pointer;        
        font-size: 16px;    
        `;  
    const info = document.createElement('div');  
    info.innerHTML =  `  
        <h6>수원 화성의 서문</h6>  
        <p>대한민국의 보물 제403호</p>  
    `;  
    content.appendChild(closeButton);  
    content.appendChild(info);  
    const overlay = new window.kakao.maps.CustomOverlay({  
        content,  
        position: suwon,  
  
        //커스텀 좌표도 만들기 center는 0.5 / 0.5        
        xAnchor: 0.3,  
        yAnchor: 0.9  
    });  
    overlay.setMap(maps);  
    setMyOverlay(overlay);  
    closeButton.addEventListener('click', ()=>{  
        overlay.setMap(null);  
    })  
}
```


#### 스크립트 로드 후, map그리기

```tsx
mapScript.onload = ()=>{  
    window.kakao.maps.load(drawMap);  
}  
document.head.appendChild(mapScript);  
  
// 나 세팅 끝났어. 라는 의미.  
setIsLoad(true);
```

setIsLoad는 현재 맵이 로드된 상태라는 것을 의미하는데, 나중에 로드가 되었는지의 여부를 파악하기 위해서 사용할 수 있다. 
당연히 위 코드도 useEffect내부에 넣는다.
처음에 실행될 때 필요하기 때문이다.



---


### 검색기능 구현하기

```tsx
const searchMap = (searchKeyword: string) => {  
    if (!isLoad || !searchKeyword) {  
        alert('지도가 로드되지 않았거나 검색어가 없습니다')  
        return false;  
    }  
  
  
    // 카카오 검색 기능 구현  
    const ps = new window.kakao.maps.services.Places();  
    ps.keywordSearch(searchKeyword, placesSearchCB);  
  
    //호이스팅 문제때문에 화살표는 동작하지않아서 function으로 지정  
    function placesSearchCB(data: any[], status: string) {  
        if (status === window.kakao.maps.services.Status.OK) {  
  
            if (myMarker) myMarker.setMap(null);  
            if (myOverlay) myOverlay.setMap(null);  
  
            // 검색된 장소 위치를 기준으로 지도 범위를 재설정하기위해  
            // LatLngBounds 객체에 좌표를 추가합니다  
            const bounds = new window.kakao.maps.LatLngBounds();  
            const searchPosition = new window.kakao.maps.LatLng(data[0].y, data[0].x); //새로운 포지션.나중에 써먹으려고 받아뒀다.  
            bounds.extend(searchPosition);  
  
            // 검색된 장소 위치를 기준으로 지도 범위를 재설정합니다  
            myMap.setBounds(bounds);  
  
  
            // 새로운 마커  
            const searchMarker = new window.kakao.maps.Marker({position: searchPosition});  
            searchMarker.setMap(myMap);  
            setMyMarker(searchMarker);  
  
            // 새로운 오버레이  
            const content = document.createElement('div');  
            content.id = "customOverlay";  
            content.style.cssText = `  
                background: white;                border: 1px solid #ccc;                border-radius: 8px;                padding: 10px;                font-size: 13px;                position: relative;            `;  
            const closeButton = document.createElement('div');  
            closeButton.id = "customCloseButton";  
            closeButton.innerHTML = 'X';  
            closeButton.style.cssText = `  
                position: absolute;                right: 10px;                top: 10px;                cursor: pointer;                font-size: 16px;            `;  
            const info = document.createElement('div');  
            info.innerHTML = `  
                <h6>${data[0].place_name}</h6>  
                <p>${data[0].address_name}</p>  
            `;  
            content.appendChild(closeButton);  
            content.appendChild(info);  
            const overlay = new window.kakao.maps.CustomOverlay({  
                content,  
                position: searchPosition,  
  
                //좌표도 만들기 center는 0.5 / 0.5                xAnchor: 0.3,  
                yAnchor: 0.9  
            });  
            overlay.setMap(myMap);  
            setMyOverlay(overlay);  
  
            closeButton.addEventListener('click', () => {  
                overlay.setMap(null);  
            })  
        }  
  
    }  
}
```


### return

```tsx
return (  
    <div className={styles.mapbox}>  
        <h2>Keyword Map Search</h2>  
        <div className={styles.btns}>  
            {  
                keywordBtns.map((btn, idx) => (  
                    <button key={idx} onClick={()=> setKeyword(btn)}>{btn}</button>  
                ))  
            }  
        </div>  
        <div className={styles.search}>  
            <input type="text" placeholder="지역키워드를 입력해주세요"  
                   value={keyword}  
                   onChange={e=> setKeyword(e.target.value) }/>  
            <button onClick={()=>searchMap(keyword)}>Search</button>  
        </div>  
        <div className={styles.map} ref={mapRef}></div>  
  
    </div>);
```