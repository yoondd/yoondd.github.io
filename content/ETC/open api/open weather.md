https://openweathermap.org/current

어떻게 써야하는지 자세히 나와있다.

2025.04.08.
오늘 처음으로 회원가입을 했고, 간단하게 키를 발급받고 gpt를 사용해서 도움받아 구현했다.

```js
const fetchWeather = async (city) => {
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apikey}`;
  
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
      }
      const data = await response.json();
 
      // 날씨 아이콘.
      const iconCode = data.weather[0].icon;
      const iconUrl = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
      const weatherIconElement = document.getElementById('weatherIcon');
      weatherIconElement.src = iconUrl;
 
      // 온도.
      const temp = data.main.temp;
      const temp_Element = document.getElementById('temp');
      temp_Element.textContent = `${temp}°C`;

      // 날씨 컨디션. 
      const weatherCondition = data.weather[0].main; 
      const conditionElement = document.getElementById('condition');
      conditionElement.textContent = weatherCondition;
      
    } catch (error) {
      console.error(error.message);
    }
  };
  
  // 사용 예시
  fetchWeather('Suwon');
  
```