
목표: 뷰템플릿 페이지가 출력되기까지 mvc역할과 실행흐름이 뭘까?

서버에서는 모델, 뷰, 컨트롤러가 유기적으로 마구마구 동작하고있다
컨트롤러가 GetMapping()을 이용해 주문(요청)받고, 
해당 메서드가 리턴값을 통해 view페이지를 보여주더라. 
여기서 view페이지에 변수가 등장하는데, 여기까지 도달하기위해 model을 거쳐야하더라.
변수는 model을 통해 등록하니까말이야. !

같은 방법으로 한번 더 해봤어.

```java
@Controller
public class FirstController {

    @GetMapping("/hi")
    public String niceToMeetYou(Model model){
        model.addAttribute( "username","혜경" );
        return "greetings"; // templates안의 greetings.mustache를 브라우저로 전송해라.
    }

    @GetMapping("/bye")
    public String seeYouNext(Model model){
        model.addAttribute("nickname", "홍길동");
        return "goodbye";
    }

}

```