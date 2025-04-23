여러 필수코드를 단순화 할 수 있고, 코드 Refactoring(코드개선)가능 println메서드를 → Logging(자동차 블랙박스처럼)가능

몰라도 문제는 없지만 번거로운 절차들을 자동화할 수 있어서 유용하다

# 설치

1. build.gradle의 dependencies 가 중요한데, 우리가 설치했던 내용들이 여기 다 나온다.
2. dependencies에 lombok이라는 것을 추가해주자

```java
//롬복 추가
compileOnly 'org.projectlombok:lombok'
annotationProcessor 'org.projectlombok:lombok'
```

3. 우측 위에 load gradle changes 코끼리버튼을 누른다
4. 플러그인도 설치하자 - lombok
5. 어노테이션 설치 활성화
    
    ![](https://i.imgur.com/Ij6ZIa0.png)


---

여기까지가 lombok 설치다. 원래는 맨처럼 io 때 해야하지만, 이렇게 나중에 해도돼

도움말-액션찾기-플러그인에서 잘 설치되었는지 확인도 할 수 있어

# 리팩토링

잘 설치했으면,

```java
@AllArgsConstructor
@ToString
```

이런걸 dto같은데 넣어주면된다

# 로깅 @Slf4j

로깅은 블랙박스같은 느낌이다. 매순간의 영상을 다 기록하듯이, 내가 확인하고싶은 서버의 사건들을 모두 기록해둘 수 있는 기능이라고 볼 수 있다.

```java
@Controller
@Slf4j //로깅을 위한 골뱅이(어노테이션) simple loging for 4 java
public class ArticleController {

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){
        //System.out.println(form.toString()); 로깅 기능으로 대체해보자
        log.info(form.toString());

        return "";
    }
}
```

위처럼 slf4j를 이용하면, System.out.println()이 아니라,

[log.info](http://log.info)(대상)을 쓸 수 있다