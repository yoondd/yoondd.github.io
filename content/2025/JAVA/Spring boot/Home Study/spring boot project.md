
- 기본적으로 세팅된 환경아래, `resources/static`폴더에 hello.html을 넣고 서버를 돌리면, localhost:8080/hello.html으로 바로 접근도 가능하다

1. templates에 view페이지 만들기
2. 컨트롤러 선언 @Controller
3. 뷰 페이지를 반환하는 메서드 선언  @GetMapping으로 라우팅
4. 변수를 넘길 수있도록 Model부르기. 