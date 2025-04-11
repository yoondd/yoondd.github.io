```js
const fetchWeather = async (city) => {
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=0f347a891b27262bf15023a506af4bbe`;
  
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
      }
      const data = await response.json();
 
      // 온도.
      const temp = data.main.temp;
      const temp_Element = document.getElementById('temp');
      temp_Element.textContent = `${temp}°C`;

      
    } catch (error) {
      console.error(error.message);
    }
  };
  
  // 사용 예시
  fetchWeather('Suwon');
  
  
```

# fetch 

fetch는 네트워크 요청을 보내고 결과로 promise객체를 반환한다
이 promise는 요청이 완료되면 응답 객체(Response)로 해결된다.
http 상태코드, 헤더, 본문 데이터에 접근 할 수 있는 메서드를 제공한다.


## promise?

`Promise`는 비동기 작업의 결과를 처리하는 객체로, 콜백 중첩 대신 `.then()` 체인을 사용하여 가독성을 높인다. 결국 promise는 ==콜백지옥을 해결해준다==. 비동기를 동기화 하기 위해 사용하는 객체다. ==실행은 바로 하되 결과값은 나중에 받는 객체라고 볼 수 있다.== 

[[Promiss]]


# await

비동기 함수(async)에서만 사용할 수 있는 것으로, promise가 처리될 때까지 코드 실행을 일시중단한다. \
promise가 해결되면 그 결과값을 반환하고 이후 코드를 계속 실핸한다.


## 왜 fetch에서 await가 두번이 필요하지? - 두번 대기해야한다??

1. 네트워크 요청 전송 및 ==응답 헤더 받을 때까지 대기==
	첫번째 await fetch(url)는 네트워크 요청을 보낸다.
	또, 서버로부터 응답헤더를 받을 때까지 기다린다.
	여기서는 아직 응답본문은 처리되지 않는다. - 따라서 반환된 응답객체에는 본문데이터를 읽기위한 메서드만 포함되어 있다.

2. 본문 데이터를 읽고 ==파싱==
	두번째 await response.json()은 응답 본문 데이터를 읽고 파싱하는 작업을 한다.
	본문 데이터는 스트림 형태로 제공되기때문에 이를 JSON으로 변환하려면 추가 비동기 작업이 필요하다. 그러니까 한번 더 await로 대기해야한다.


---

# code example

```js
async function fetchData(url) {
  const response = await fetch(url); // 첫 번째 await: 응답 헤더를 기다림
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  const data = await response.json(); // 두 번째 await: 본문 데이터를 JSON으로 변환
  console.log(data);
}

fetchData('https://api.example.com/data');

```


---

내 코드를 then방식으로 변환한다면?

```js
const fetchWeather = (city) => {
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${api키가 들어가는 부분}`;
  fetch(url)
    .then((response) => {
      if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
      }
      return response.json(); // 응답 본문을 JSON으로 변환
    })
    .then((data) => {
      // 날씨 아이콘
      const iconCode = data.weather[0].icon;
      const iconUrl = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
      const weatherIconElement = document.getElementById('weatherIcon');
      weatherIconElement.src = iconUrl;

      // 온도
      const temp = data.main.temp;
      const temp_Element = document.getElementById('temp');
      temp_Element.textContent = `${temp}°C`;

      // 날씨 컨디션
      const weatherCondition = data.weather[0].main;
      const conditionElement = document.getElementById('condition');
      conditionElement.textContent = weatherCondition;
    })
    .catch((error) => {
      console.error(error.message); // 에러 처리
    });
};

// 사용 예시
fetchWeather('Suwon');

```
