

```java
import React, { useEffect, useState, useRef } from 'react';  
import styles from '@/styles/Map.module.scss';    
```


```java

// 전역변수 선언  
declare global {  
  
    //리액트 전용 Window - window.open 처럼 자바스크립트에서 사용하던 기능과는 별개  
    interface Window {  
        kakao: any; //나의 window에서는 kakao가 존재한다.  
    }  
}  
  
const Map = () => {  
  
  
    //----------변수만들기  
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
  
  
  
    useEffect(()=>{  
        //자바스크립트부터 들고오자 ---------------- service라이브러리 불러오기 https://apis.map.kakao.com/web/guide/        const mapScript = document.createElement('script');  
        // autoload=false를 넣어서 내가 원할때 로딩될 수 있게 만든다  
        mapScript.setAttribute('src', `//dapi.kakao.com/v2/maps/sdk.js?appkey=${process.env.NEXT_PUBLIC_KAKAO_JS_KEY}&autoload=false&libraries=services`);  
        mapScript.async = true; //들고오는 객체 자체에 fetch가 들어가기때문에 무조건 넣어야한다.  
        // -----------여기까지 들고오면 이제 샘플에있는걸 편안하게 사용할 수 있다  
  
  
        const drawMap = () => {  
            const center = new window.kakao.maps.LatLng(37.2855250, 127.0076167);  
            const options = {  
                center, // center: center와 같다.  
                level: 3  
            }  
            //지도생성: 카카오에서 주는 기능으로 하나 만들고  
            const maps = new window.kakao.maps.Map(mapRef.current, options);  
            setMyMap(maps); //내가만든 맵에다가 kakao가 만든 맵 넣어주기.  
  
            //교통상황생성  
            maps.addOverlayMapTypeId(window.kakao.maps.MapTypeId.TRAFFIC);  
  
            //마커생성  
            // const markerPosition = new window.kakao.maps.LatLng(37.2855250, 127.0076167);  
            const marker = new window.kakao.maps.Marker({ position: center });  
            marker.setMap(maps);  
            setMyMarker(marker);  
  
            //초기셋 오버레이 생성  
  
        }  
  
        mapScript.onload = ()=>{  
  
  
            window.kakao.maps.load(drawMap);  
  
        }  
  
    },[])  
  
  
  
  
  
  
  
  
    //내가 구현하는 작업 하나  
    return (  
        <div>  
  
        </div>    );  
};  
  
export default Map;
```
