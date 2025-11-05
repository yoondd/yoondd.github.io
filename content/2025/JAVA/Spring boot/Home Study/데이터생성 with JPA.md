JPA를 이용해 데이터베이스를 생성하자

자바를 모르는 db에게 설명해야한다

![](https://i.imgur.com/x8alPnv.png)


entity와 repository가 필요하다

서버에서 db로 넘길수 있도록!.

 entity: 규격화된 데이터  

![](https://i.imgur.com/SehDSFA.png)



이 과정을 통해야만 데이터베이스에 담을 수 있다.

1. dto를 entity로 변환
2. entity를 repository로 변환
3. database로 쌓기

(2번부터 3번으로 가게 해주는 애가 JPA)

1. 엔티티 패키지를 나의 프로젝트 메인패키지 안에 만들고
    
    /Users/yunhyegyeong/Developer/firstproject/src/main/java/com/example/firstproject/entity/Article.java
    
2. 그안에 클래스를 만들어
    - 클래스 내부에서는 어노테이션으로 Entity라는 것을 적어줘야해

3. 컨트롤러 내부에서는 두가지 단계를 이루어야해

4. dto 를 반환해서  entity 를 만든다
    - dto에게 toEntity()라는 메서드가 있어야해
    
    ```java
     public Article toEntity() {
            return new Article(null, title, content);
     }
    ```
    
    - entity 안에 있는 아이의 모습 ㅜㅜㅜ
        
        ```java
        package com.example.firstproject.entity;
        
        import jakarta.persistence.Column;
        import jakarta.persistence.Entity;
        import jakarta.persistence.GeneratedValue;
        import jakarta.persistence.Id;
        
        @Entity //이래먀만 db가 해당 객체를 인식할 수 있다. 아래 클래스로 테이블을 만들겠다 라는 의미
        public class Article {
        
            //private Long id; //구분하기위한 대표값(기본키)
            @Id
            @GeneratedValue
            private Long id;
        
            @Column //db에서 관리하는 테이블 쪽에 연결될 수 있도록
            private String title;
        
            @Column
            private String content;
        
            public Article(Long id, String title, String content) {
                this.id = id;
                this.title = title;
                this.content = content;
            }
        
            @Override
            public String toString() {
                return "Article{" +
                        "id=" + id +
                        ", title='" + title + '\'' +
                        ", content='" + content + '\'' +
                        '}';
            }
        }
        
        ```
        

5. repository에게 entity를 db안에 저장하게 한다

- repository 패키지를 만든다
- 그 안에 인터페이스로 하나를 새롭게 만든다 ArticleRepository
- 내부 내용으로 extends CrudRepository<a,b>{
- a에는 관리대상 entity, b는 관리대상 entity에서 대표값의 타입
- 그래서 우리는 <Article, Long> 이 된거다

```java
 @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){
        System.out.println(form.toString());

        //1. dto를 변환 entity!
        Article article = form.toEntity();
        System.out.println(article.toString());

        //2. repository에게 entity를 db안에 저장하게 함
        Article saved = articleRepository.save(article);
        System.out.println(saved.toString());
        return "";
    }
```