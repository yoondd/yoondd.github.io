
## 데이터 수정 폼 만들기

생성과 조회를 했으니 이제는 수정을 해보자
데이터 상세페이지에 수정하기 기능만들기

![](https://i.imgur.com/t9hcbwf.png)



1. 데이터 상세페이지에서 수정하기 링크 작성
    
    ```java
    <a href="/articles/{{article.id}}/edit" class="btn btn-primary">Edit</a>
    ```
    
2. 컨트롤러에서 각각의 설정을한다
    
    ```java
    @GetMapping("/aricles/{id}/edit")
        public String edit(@PathVariable Long id, Model model){
            //수정할 데이터를 먼저 가져오자 
            Article articleEntity = articleRepository.findById(id).orElse(null);
            
            //모델에 데이터를 등록 !!
            model.addAttribute("article", articleEntity);
            
            //view page
            return "articles/edit";
        }
    ```
    
    - view page로 연결되게 한다
    - 수정할 데이터를 repository를 이용해서 가져온다 — 여기서 @PathVariable 이용하는거 잊지말자!
    - 모델에 데이터를 등록하자 model도 불러오고
3. edit.mustach파일 만들기 ( create파일과 비슷하게 생겼으니까 그대로 가져와)
    
4. 잘 나올 수 있게 edit파일을 제작하자
    
    ```java
    {{>layouts/header}}
    
    {{#article}}
    <form action="" method="post" class="container py-5">
        <div class="mb-3">
            <label class="form-label">title</label>
            <input type="text" class="form-control" name="title" value="{{title}}">
        </div>
        <div class="mb-3">
            <label class="form-label">content</label>
            <textarea name="content" rows="3" class="form-control">{{content}}</textarea>
        </div>
        <button class="btn btn-primary">submit</button>
        <a href="/articles/{{id}}" class="btn btn-secondary">Back</a>
    </form>
    {{/article}}
    
    {{>layouts/footer}}
    
    ```


---



## 데이터 수정하기

수정된 데이터를 데이터 베이스로 갱신하고 이를 확인해보자.

자꾸 껐다키는데 데이터를 생성해야하니까 귀찮아

더미데이터를 만들자 (/Users/yunhyegyeong/Developer/firstproject/src/main/resources/data.sql)

```java
INSERT INTO article(id, title, content) VALUES (1, "봄", "꽃이 활짝.");
INSERT INTO article(id, title, content) VALUES (2, "여름", "바다는 푸르다.");
INSERT INTO article(id, title, content) VALUES (3, "봄", "산이 물들어요.");
```

→ 고생은 했지만 안돼. 버전이 지원을 하지않나봐.

그래서 그냥 제끼고.

```java

    @PostMapping("/articles/update")
    public String update(ArticleForm form){
        log.info(form.toString());

        //dto-> entity
        Article articleEntity = form.toEntity();
        log.info(articleEntity.toString());

        //entity --> db
            //수정하는거니까, 있는걸 바꿔야한다 db에서 기존 데이터를 가져온다
            Article target = articleRepository.findById(articleEntity.getId()).orElse(null);

            //기존데이터의 값을 갱신한다 .. db에서 확인해봐 진짜야
            if(target!=null){
                articleRepository.save(articleEntity);
            }

        //redirect
        return "redirect:/articles/" + articleEntity.getId();
    }
```