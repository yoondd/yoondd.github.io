
데이터 삭제를 구현하고 직접 확인하라

흐름

1. 삭제 요청전달
2. 디비에서 찾아서 삭제
3. 리다이렉트보냄

![](https://i.imgur.com/gvaSGhI.png)

dto는 필요없어. 어차피 url 라우팅정보야.

지우는거라서 entity등장할 필요도 없어

1. 상세 페이지에서 delete버튼을 만들어라
    
    ```java
    <a href="/articles/{{article.id}}/delete" class="btn btn-primary">Delete</a>
    ```
    
    deleteMapping같은걸 html을 지원하지않으니까 그냥 get방식을 이용하려고 a태그를 썼어.
    
2. 이제 controller 에서 할일을 설정해야겠어
    
    ```java
     @GetMapping("/articles/{id}/delete")
        public String delete(@PathVariable Long id){
            log.info("삭제 요청 ");
            
            //삭제 대상 가져온다
            Article target = articleRepository.findById(id).orElse(null);
            log.info(target.toString());
            
            //대상을 삭제 한다
            if(target!=null){
                articleRepository.delete(target);
            }
    
            //삭제 완료 되었으면 결과 페이지로 리다이렉트 한다
            return "redirect:/articles";
        }
    ```
    
    db랑 일할때는 항상 repository를 이용해
    
3. 삭제 완료 메시지 정도 남겨주는게 좋겠지. - RedirectAttributes: 일회성으로 삭제메시지 띄우기 가능
    
    ```java
     @GetMapping("/articles/{id}/delete")
        public String delete(@PathVariable Long id, RedirectAttributes rttr){
            log.info("삭제 요청 ");
            //1. 삭제 대상 가져온다
            Article target = articleRepository.findById(id).orElse(null);
            log.info(target.toString());
            //2. 대상을 삭제 한다 (있는지를 먼저 확인안하면 시스템이 스탑된다)
            if(target!=null){
                //2-1 
                articleRepository.delete(target);
                //2-2 한번 쓰고 사라지는 휘발성으로 하나 만들어준다
                rttr.addFlashAttribute("msg","삭제가 완료되었습니다!");
            }
    
            //3. 삭제 완료 되었으면 결과 페이지로 리다이렉트 한다
            return "redirect:/articles";
        }
    ```
    
    위처럼 주석을 잘 적어주면 아주 편해.
    

- 레이아웃쪽 header한테도 알럿창 하나 넣어주고.

```java
<!--alert msg-->
{{#msg}}
    <div class="alert alert-primary alert-dismissible">
        {{msg}}
        <button class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
    </div>
{{/msg}}
```