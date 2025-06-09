http는 OSI 7계층(응용계층)에서 쓰는 프로토콜이다

통신 프로토콜이다. 통신아니었으면 애초에 필요가 없었겠지.

원래 목적은 웹에서 자료를 넘기고 받기 위한 목적이었으나,

지금은 ==JSON을 주고받기위함==으로 확장이 되었다고 보면된다



http통신의 기본 포트는 80.

따라서 우리는 `http://naver.com`이런식으로 입력해도 알아서 연결이 되었던 것이다

우리가 작업을 할때, `http://localhost:8080`이라고 했던 이유는 기본포트80이 아닌 8080으로 접속하기위함이다.



### http요청은 비연결성 즉, 일회성이라고 볼 수 있다

비연결성의 문제는 무엇이냐, 내가 누구인지를 알려주는 '인증'이다

그래서 할 수 없이 token이라는 것을 이용하여 나라는 것을 저장하고는 했던 것이다



서버와 클라이언트는 단순히 브라우저냐 아니냐가 아니다

클라이언트를 요청자고, 서버는 데이터를 주는 사람이라고 생각하는 것이 편하다

그러니 클라이언트는 브라우저가 아니라 앱이나 다른 하나의 서버일수도 있다는 소리다


## curl이란?

```curl
GET / HTTP/1.1
Host: www.naver.com:443
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
sec-ch-ua: "Chromium";v="92", " Not A;Brand";v="99", "Google Chrome";v="92"
sec-ch-ua-mobile: ?0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36
```

이런식으로 넘어오는데, 이걸 우리는 curl이라고 부른다

카카오 api에서 rest api에 보면 이런 내용이 있다.

애초에 가이드로 curl으로 보내준다

우리도 통신에 대해 공부하려면 이 curl을 읽을 수 있어야한다

웹이란게 결국 데이터를 주고받는 기술이니까 말이다

***http는 웹이 아니라 그저 데이터통신을 말한다는 것이다***

앱을 통해서 데이터를 주고받는것도 전부 http통신이다


#### mapping와 프로토콜

자, 그러면 하나하나 뜯어가면서 배워볼까?

`GET / HTTP/1.1`

여기서 나오는 GET은 바로 mapping을 말한다

왜 이걸 명시하는 것일까? 
	사람이 웹브라우저를 통해 데이터를 주고받게 되면, 메서드를 get, post밖에 못쓴다. 그저 body를 붙일까 말까 이거하나만 구분하겠다는 소리다. deleter매핑일경우 body가 없으니까 get을 사용해야한다는소리다. 이때문에 어쩔 수 없이 curl의 맨 앞쪽에 이걸 선언해야한다. 

그다음에 나오는 HTTP/1.1은 바로 프로토콜이다


#### 도메인과 포트

그다음 `Host: www.naver.com:443`은 바로 도메인이다. 

