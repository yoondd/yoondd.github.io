[[ResponseService]]

성공메시지 작업은 끝났으니까!

자, 이제는 실패코드를 만들어보자.

성공코드는 아무것도 받지 않아도 괜찮았지만,  실패는 다르다. 

실패의 상황은 너무나 많다. 따라서 실패의 사유를 받아줘야한다.

```java
@Service  
public class ResponseService {  
    public static CommonResult getSuccessResult() {  
        CommonResult result = new CommonResult();  
        result.setIsSuccess(true);  
        result.setCode(ResultCode.SUCCESS.getCode());  
        result.setMsg(ResultCode.SUCCESS.getMsg());  
  
        //값이 다 채워진걸 내뱉어 준다.  
        return result;  
    }  
  
    public static CommonResult getFailResult(ResultCode resultCode) {  
        CommonResult result = new CommonResult();  
        result.setIsSuccess(false);  
        result.setCode(resultCode.getCode());  
        result.setMsg(resultCode.getMsg());  
  
        return result;  
    }  
}
```

이렇게 모양을 만들었으나, 중복된 코드가 눈에 띈다.

이런 중복코드는 정말 너무나도 싫다.

그래서 잠시 공부했다 -- [[C의 포인터]]




## 포인터를 이용해 멋진 코드로 변경해보자


자, 그러면 이제 코드를 깔끔하게 바꾸어보자.

음식 리필하는 것과 똑같기때문에 그릇을가져가서 채워달라고 부탁해야한다.

그릇 채워주는 애를 이제부터 고용하자.

보이면 안되는 친구야. 그러니까 **private**로 만들 것이다.

```java
private static void setResult(CommonResult commonResult, boolean isSuccess, ResultCode resultcode){  
  
}```

엥. private인데 static이라고? 

서로 알게하기위해선 어쩔수가 없어. 메모리의 어느영역에 올라가는가가 더 중요하거든.

위에 static인 애들이 얘를 어떻게 알겠어. 그러니까 얘도 private지만 static으로 올려.


매개변수의 내용을 보면

그릇주세요 -- CommonResult commonResult

성공여부확인 -- boolean isSuccess

에러상황 -- ResultCode resultcode


```java
private static void setResult(CommonResult commonResult, boolean isSuccess, ResultCode resultcode){  
    commonResult.setIsSuccess(isSuccess);  
    commonResult.setCode(resultcode.getCode());  
    commonResult.setMsg(resultcode.getMsg());  
}
```

즉 그릇을 가져가서 그릇을 채워오는 느낌의 메서드라고 보면 된다.

바로 이게 포인터를 사용하는 이유다. 

C의 핵심기술이지. 포인터는 이렇게 쓰는거야.

이해가 안되면 다시한번 읽어봐 - [[C의 포인터]]


```java
@Service  
public class ResponseService {  
    public static CommonResult getSuccessResult() {  
        CommonResult result = new CommonResult();  
        setResult(result, true, ResultCode.SUCCESS);  
        return result;  
    }  
  
    public static CommonResult getFailResult(ResultCode resultCode) {  
        CommonResult result = new CommonResult();  
        setResult(result, false, resultCode);  
        return result;  
    }  
  
    private static void setResult(CommonResult commonResult, boolean isSuccess, ResultCode resultcode){  
        commonResult.setIsSuccess(isSuccess);  
        commonResult.setCode(resultcode.getCode());  
        commonResult.setMsg(resultcode.getMsg());  
    }  
}
```

와. 진짜 코드가 아름다워졌다.

포인터를 활용해서 아주 아름다운 에러처리 서비스를 만들어냈다.



메모리 접근하는 본질자체는 똑같지만, 안전장치를 둔다.



#### 오버로딩을 사용해서 기본실패도 만들어줘

```java
// default fail  
public static CommonResult getFailResult() {  
    CommonResult result = new CommonResult();  
    setResult(result, false, ResultCode.FAILED);  
    return result;  
}  
  
public static CommonResult getFailResult(ResultCode resultCode) {  
    CommonResult result = new CommonResult();  
    setResult(result, false, resultCode);  
    return result;  
}
```


진짜 중복코드 싹 빼고,

아주 깔끔하게 예쁘게 만들었다.

