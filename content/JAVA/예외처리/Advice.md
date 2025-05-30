던져진 에러를 받아서 메시지처리해줄 사람이 필요하다.

중간에 *조언자* 가 끼는 느낌이다.


## 1. `advice` 패키지 추가

model.. service.. 다른 애들과 같은 레벨에 advice 패키지를 추가한다


---

## 2. `advice/ExceptionAdvice` 생성

예외에 대해 advice를 주는 아이.

이 전에 먼저 어노테이션 동작구조를 알아야한다.

[[어노테이션 동작구조]]


### `@RestControllerAdvice`

advice는 advice인데 앞에 restcontroller가 붙었네.

그건 json으로 뽑아주는앤데? 

그냥 contoller는 html로 뿌려주니까, 어떤 파일로 가면된다 - 하고 길만 다시 알려주면되는데.

restconroller니까 json으로 주게된다.

가이드 해주는 방향이 달라서 이렇게 나누어져 있다.

우리는 당연히 json을 사용할꺼니까 저걸 쓰는 것이다.



```java
//기본 어드바이스
@RestControllerAdvice  
public class ExceptionAdvice {  
    @ExceptionHandler(Exception.class)  
    @ResponseStatus(HttpStatus.BAD_REQUEST)  
    protected CommonResult handleException(HttpServletRequest request, Exception ex) {  
        return ResponseService.getFailResult(ResultCode.FAILED);  
    }  
}
```

서비스에서 별도로 지정하지 않는 경우에는, 디폴트라서 대부분 여기로 들어온다. 

고객이말하는 에러는 request에 들어오고 나중에 로그에 쌓을려면 ex를 보낸다. 

시스템을 견고하게 만드는 핵심이다


---


## 3. 무슨 기준으로 만드나?

```java
@RestControllerAdvice  
public class ExceptionAdvice {  

	//1번....
    @ExceptionHandler(Exception.class)  
    @ResponseStatus(HttpStatus.BAD_REQUEST)  
    protected CommonResult handleException(HttpServletRequest request, Exception ex) {  
        return ResponseService.getFailResult(ResultCode.FAILED);  
    }  
  
	//2번...
    @ExceptionHandler(CMissingDataException.class)  
    @ResponseStatus(HttpStatus.BAD_REQUEST)  
    protected CommonResult handleCMissingDataException(HttpServletRequest request, CMissingDataException ex) {  
        return ResponseService.getFailResult(ResultCode.MISSING_DATA);  
    }  
      
}
```

1번.. 2번... 이런식으로 만들면되는데, 

오버로딩으로 여러개를 만들었잖아? 

Custom Exception추가할때마다 이거를 하나씩 만들어야한다. - ==1:1 짝꿍==이다

유형별로 잘 추가해두어야한다


---



## 기억해야할 것

필요하지않아도 매개변수자리에다가 Exception e도 추가해주기. 나중에 로그쌓을 수도 있으니까~


---


## 제네릭은 안되나요?

할 수가 없다. 제네릭은 런타임시기에 작동하는거잖아.

그런데 어드바이스는 컴파일시점에 이미 필요하단말이야

즉, 비상출구는 빌드때 이미 필요한데, 제네렉은 런타임시기에 써야하는거니까 

그걸 쓸 수 없이 미리 준비하는 것이 필요하다