> 자바스프링부트로 백을 만들고 next js로 프론트를 만들어 구현하려고하니 에러가났다. 서로를 이해하지 못하는 에러였다. 


## 문제상황

프론트엔드와 백엔드를 "아예 다른 폴더"에서 개발하고, 각각 독립적으로 서버를 띄워서(예: 프론트는 localhost:3000, 백엔드는 localhost:8080 등) 통신한다면, CORS(Cross-Origin Resource Sharing) 설정이 반드시 필요하다. 

브라우저의 CORS 정책은 "도메인, 포트, 프로토콜"이 하나라도 다르면 서로 다른 오리진(origin)으로 간주하고, 이 경우 백엔드에서 명시적으로 허용하지 않으면 브라우저가 요청을 차단한다. 
즉, 프론트엔드가 별도의 폴더에서 별도의 서버(포트)로 띄워져 있다면, 아래와 같은 상황이다.

- 예시:
    - 프론트엔드: [http://localhost:3000](http://localhost:3000/)
    - 백엔드: [http://localhost:8883](http://localhost:8883/)
        

이렇게 오리진이 다르면, 프론트에서 백엔드로 API 요청(POST, GET 등)을 보낼 때 브라우저가 CORS 오류를 발생시킨다.
따라서, 백엔드(Spring 등)에서 CORS 허용 설정을 해주지 않으면 정상적으로 통신이 불가능하다. 


## 해결법

사실 나도 엄청 열심히 찾아봐서 controller에 어노테이션을 넣어봤는데 그로도 해결이 되지않아 옆친구의 도움을 받았다. 

com.example.coding폴더에 config라는 폴더를 만든다.
그리고 config 내부에 WebConfig파일을 만들어 다음과 같이 세팅한다.

```java
package com.example.coding.config;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.web.servlet.config.annotation.CorsRegistry;  
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;  
  
@Configuration  
public class WebConfig {  
  
    @Bean  
    public WebMvcConfigurer corsConfigurer() {  
        return new WebMvcConfigurer() {  
            @Override  
            public void addCorsMappings(CorsRegistry registry) {  
                registry.addMapping("/**") // 모든 경로  
                        .allowedOrigins("http://localhost:3000", "http://192.168.0.60:3000")  
                        .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")  
                        .allowedHeaders("*")  
                        .allowCredentials(true);  
            }  
        };  
    }  
}
```

명시적으로 넣어두는 것이다. 
이로써 두 폴더가 잘 통신하는 것을 알 수 있었다.