ResponseService는 바로 롯데백화점 쇼핑백같은 존재다.

내용물은 각 코너마다 다르지만, 모두 롯데백화점 쇼핑백으로 씌워져 있기때문이다.

품격을 살게하기위해 모두 같은 쇼핑백에 통일해서 보여준다.

하나의 서비스-컨트롤러에서만 쓰기위해 만드는게 아니라는 소리다.

자, 품격있는 코드를 볼 준비가 되었는가.



## 품격있는 쇼핑백 - ResponseService


```java
@Service  
@RequiredArgsConstructor  
public class Test2Service {  
  
    public String getTestName(){  
        return "yoon";  
    }  
}
```

앞선 예제와 아무상관없는 별도의 서비스를 만들었다


```java
@RestController  
@RequiredArgsConstructor  
@RequestMapping("/test2")  
public class Test2Controller {  
  
    private final Test2Service test2Service;  
  
    @GetMapping("/name")  
    public SingleResult<String> getTestName(){  
        return ResponseService.getSingleResult(test2Service.getTestName())  
    }  
}
```

그냥 글자하나 보내는 이 작업을 가지고 

품격있게 업그레이드를 해버렸다


미쳤다....



