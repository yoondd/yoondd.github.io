mvc패턴을 활용한 템플릿 페이지를 만드는 것이 이번 챕터의 목표.

자바는 다 정해져있어. 다. 전부.

웹페이지에 변수를 허용하는 뷰 템플릿.

뷰템플릿: 화면을 담당하는 기술. 웹페이지를 하나의 틀로 만들고, 변수의 값에 따라서 수많은 페이지로 바뀔 수 있다. controller(처리과정 담당/ 어떻게 오면..뭐 하고…)와 model(값, 데이터베이스 담당영역)이라는 동료가 있어.

1. 뷰템플릿이 어디있을까?

```jsx
/Users/yunhyegyeong/Developer/firstproject/src/main/resources/templates
```

1. greetings.mustach를 이 폴더에다가 만들면되는데, 그전에 도움말-액션찾기에서 plugins를 입력하고 mustache를 설치를해. 그리고나서 다시 만들면돼.
    - mustach가 뭐냐고? 그냥 html을 다른이름으로 부른다고 생각해도돼
    - doc 하고 tab
2. 자!!! **이제 라우팅을 거는거야**(컨트롤러) - 규정한 위치가 있어
3. com.example.firstproject 폴더에 package추가
4. 새로생긴 폴더 내부에 FirstController라는 Java 클래스 만들자고.

![](https://i.imgur.com/wmqkpsU.png)


일반적으로 블라블라 컨트롤러라고 많이 만들거든.(관례야)

1. 자 이제, 아까만든 mustache랑 연결해줄거야. 잘 봐야돼.
2. 먼저 어노테이션으로 @Controller라는 것을 명시해.

```java
@Controller
public class FirstController {
    public String niceToMeetYou(){
        return "greetings"; // templates안의 greetings.mustache를 브라우저로 전송해라.
    }
}
```

- 문자열이 아니야. 주석에 적힌것처럼 templates/greetings.mustache를 부르는 메서드를 만들어준거야.

1. 끝이아니야. 이제 라우팅을 걸어야지!

```java
package com.example.firstproject.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class FirstController {
    
    @GetMapping("/hi")
    public String niceToMeetYou(){
        return "greetings"; // templates안의 greetings.mustache를 브라우저로 전송해라.
    }
    
}
```

1. 다 되었으면 이제 재시작을 무조건해야해! 이제 hi로 접속하면 잘 나올거야.
2. 별 의미가 없어보이냐? 그러면 변수를 한번 이용해보자고. mustache를 바꿔버리자.

```html
<body>
    <h1>{{username}}}님, 반갑습니다.</h1>
</body>
```

1. 오, 이제 모델을 사용해야돼. 어디있냐고? 컨트롤러에서 받아와야해.

```java
package com.example.firstproject.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

//이게 컨트롤러
@Controller
public class FirstController {

    @GetMapping("/hi")                  //요아래 모델
    public String niceToMeetYou(Model model){
        model.addAttribute( "username","혜경" );
        
        //이게 뷰템플릿 부르기
        return "greetings"; // templates안의 greetings.mustache를 브라우저로 전송해라.
    }

}

```

- Model model을 인자로 받을라고 하니까 빨간색이 뜨더라고. 그러면 눌러서 그 프레임워크를 불러오겠다고 지정을 해주면 돼.

![](https://i.imgur.com/szG8kXt.png)
