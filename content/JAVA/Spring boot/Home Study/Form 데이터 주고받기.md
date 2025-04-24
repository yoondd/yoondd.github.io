
사용자로부터 폼데이터를 받고 이를 컨트롤러에서 확인하는 것이 목표.

이제 게시판 만들기를 시작해볼까!?

Create, Read, Update, Delete

컨트롤러 앞에 DTO를 붙여줘야한다.

쿼리나 뭐나 정보가 날라붙어올때, 그걸 담는 놈인 DTO를 붙여줘야한다

즉, 컨트롤러가 받기전에 DTO를 설계해줘야한다.

클라이언트 → 서버 → 데이터베이스

# 클라이언트가 값을 입력하고 DTO가 그 값을 받도록 하기

1. articles/new.mustache를 만들고 그 안에서 form태그를 만들었어 데이터 보낼 수 있게
    
    - 액션과 메서드를 잘 적었어.
    - 만약에 여기서 1.jpg를 불렀어. 그러면 걔는 어디있어야할까? - static폴더😍
2. 컨트롤러쪽에서 postmapping()
    
3. dto패키지만들기 ArticleForm 클래스 만들기
    
    - 던져진 데이터들을 받는 아이: DTO
    - 어디에만들어?

![](https://i.imgur.com/2qpz3N2.png)

    

```java
package com.example.firstproject.dto;

public class ArticleForm {
    private String title;
    private String content;

    public ArticleForm(String title, String content) {
        this.title = title;
        this.content = content;
    }

    @Override
    public String toString(){
        return "ArticleForm{" +
                "title='" + title + '\\'' +
                ", content='" + content + '\\'' +
                '}';
    }
}
```