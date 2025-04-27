
# 데이터 조회하기 with JPA

지금까지 공부한건 데이터 생성과정.
이제는, db에 저장된걸 화면에서 보고싶다

## 데이터 조회의 흐름

![](https://i.imgur.com/j1jL21R.png)

1. 사용자가 브라우저를 통해 데이터 요청 /article/1
2. 그 요청 url을 컨트롤러가 받아 (라우팅만 따내겠다는거다)
    - 값을 다루는게 아니라, url의 정보를 받는거니까 dto같은건 안나와.
3. 필요한 정보(url에 있겠지)를 리파지토리에게 전달해 findById(1) - 1번 레코드 찾아와
4. 리파지토리는 디비에 요청, 
5. 디비는 그걸 찾아서 엔티티로 내줘 - 컨트롤러가 먼저 받아.
6. 컨트롤러가 받은다음에 그걸 처리할걸 처리해, 이걸 모델이 받는다
7. 그거를 이제 view에서 받아서 최종적으로 view화면을 클라이언트인 브라우저로 보내줌

##  Read 만들기 과정(db조회)

1. 컨트롤러에 매핑만들기 - 우리는 /articles/1, 이런식으로 접근할거야
    
    ```java
    @GetMapping("/articles/{id}")
    ```
    
    요기 아이디 위치에 들어가는것이 변하는 수야~ 이런느낌이다(변수지정)
    

2. path에서 넘어온 변수값이라는걸 알려주기위해 어노테이션 사용
    
    ```java
    @GetMapping("/articles/{id}")
        public String show(@PathVariable Long id){
            return "";
        }
    ```
    

3. 한번 잘 들어오는지 찍어볼까?
    
    ```java
     @GetMapping("/articles/{id}")
        public String show(@PathVariable Long id){
            log.info("***id="+id);
            return "";
        }
    ```
    
    위처럼 적고나서 /articles/1000으로 접속하니 진짜로 로그창에 1000번이 잘 나오더라고.
    

4. 아이디로 데이터 찾기
    
    데이터 가져오는애는 리파지토리야.
    
    ```java
     @GetMapping("/articles/{id}")
        public String show(@PathVariable Long id){
            log.info("***id="+id);
    
            // id로 데이터 가져옴
            Article articleEntity = articleRepository.findById(id).orElse(null);
    
            return "";
        }
    ```
    
    위에있는 orElse는 값없으면 null하자는 거야.
    

5. 가져온 데이터를 모델로 등록
    
    ```java
    @GetMapping("/articles/{id}")
        public String show(@PathVariable Long id, Model model){
            log.info("***id="+id);
    
            // id로 데이터 가져옴
            Article articleEntity = articleRepository.findById(id).orElse(null);
    
            // 가져온 데이터를 모델에 등록
            model.addAttribute("article", articleEntity);
            
            return "";
        }
    ```
    

6. 보여줄 최종 페이지를 설정한다
    
    ```java
    @GetMapping("/articles/{id}")
        public String show(@PathVariable Long id, Model model){
            log.info("***id="+id);
    
            // id로 데이터 가져옴
            Article articleEntity = articleRepository.findById(id).orElse(null);
    
            // 가져온 데이터를 모델에 등록
            model.addAttribute("article", articleEntity);
    
            // 보여줄 페이지를 설정
            return "show";
        }
    ```
    

7. show.mustache파일을 제작한다
    
    이미 불러올 header, footer 들이 있으면 그런거 넣으면되지.
    
    ```java
    {{#article}} 
        <tr>
            <th scope="row">1</th>
            <td>dog</td>
            <td>so cute!</td>
        </tr>
    {{/article}}
    ```
    
    위와같이 (모델에 등록된_)article을 여기서 사용할거야~ 
    

8. 오류가 나서 할수없이 entity에 디폴트 생성자 추가
    
    ```java
    @Entity //이래먀만 db가 해당 객체를 인식할 수 있다.
    @AllArgsConstructor
    @NoArgsConstructor // 디폴트 생성자를 추가함 <-----------이거
    @ToString
    public class Article {
    ```





---



# 데이터 목록 조회하기

db속 모든 article을 조회해볼까?

단일 데이터가아닌, 여러개의데이터를 조회하고싶다

이전과 비슷하지만 반환하는데 엔티티가 아닌 리스트다(엔티티의 묶음)

![](https://i.imgur.com/Xcx7rb0.png)


1. 브라우저의 요청을 받는다
    
    [localhost:8080/articles](http://localhost:8080/articles)  라고 들어온거야
    
    ```java
    @GetMapping("/articles")
        public String index(){
            return "";
        }
    ```
    

2. 컨트롤러 내부에서 모든 아티클 가져오기 findAll()
    
    ```java
    @GetMapping("/articles")
      public String index(){
            
            //모든 아티클 가져오기
            List<Article> articleEntityList = articleRepository.findAll();
            
        }
    ```
    
    여기서 List를 써야하는데, findAll()이 반환하는 데이터는 Iterable이래. 타입을 바꿔줘야해.
    
    그러니 인터페이스에가서 Crud~~의 메서드의 리턴타입을 재정의 해줘야되네.
    
    ```java
    public interface ArticleRepository extends CrudRepository<Article, Long> {
        @Override
        ArrayList<Article> findAll();
    }
    ```
    
    [타입이 안맞으니까 해결법들이 있다면…]
    
    - casting을 해준다. = (List<Article>) articleRepository.findAll();을 이용한다.
    - Iterable타입으로 지정해버린다 Iterable<Article> articleEntityList =
    - ArrayList타입으로 반환할 수 있도록 인터페이스 자체를 수정한다

3. 가져온 아티클의 묶음을 뷰로 전달하기
    
    ```java
    @GetMapping("/articles")
        public String index(Model model){
            
            //모든 아티클 가져오기
            List<Article> articleEntityList = articleRepository.findAll();
            
            //가져온 아티클 묶음을 뷰로 전달하기
            model.addAttribute("articleList", articleEntityList);
            
        }
    ```
    

4. 뷰 페이지를 설정하기
    
    ```java
    @GetMapping("/articles")
        public String index(Model model){
            
            //모든 아티클 가져오기
            List<Article> articleEntityList = articleRepository.findAll();
            
            //가져온 아티클 묶음을 뷰로 전달하기
            model.addAttribute("articleList", articleEntityList);
            
            //뷰 페이지를 설정하기
            return "articles/index";
        }
    ```
    

5. index.mustache로 지정하기
    
    ```html
    {{#articleList}}
         <tr>
            <th scope="row">{{id}}</th>
            <td>{{title}}</td>
            <td>{{content}}</td>
         </tr>
    {{/articleList}}
    ```
    
    그냥 위처럼만 해도 알아서 안쪽 내용이 반복이된다
    
    만약에 데이터가 40개 있으면, 40번의 id,title,content가 나오는거지.





