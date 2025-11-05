```java
package com.example.baseframe.controller;  
  
import com.example.baseframe.enums.ResultCode;  
import com.example.baseframe.model.CommonResult;  
import lombok.RequiredArgsConstructor;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
@RequiredArgsConstructor  
@RestController  
@RequestMapping("/test")  
public class TestController {  
    @PostMapping("/new")  
    public String memberJoin(){  
        CommonResult commonResult = new CommonResult();  
        commonResult.setCode(ResultCode.SUCCESS.getCode());  
        commonResult.setMsg(ResultCode.SUCCESS.getMsg());  
        commonResult.setIsSuccess(true);  
        return commonResult;  
    }  
      
    @PostMapping("/admin/new")  
    public CommonResult setMember(){  
        CommonResult commonResult = new CommonResult();  
        commonResult.setCode(ResultCode.SUCCESS.getCode());  
        commonResult.setMsg(ResultCode.SUCCESS.getMsg());  
        commonResult.setIsSuccess(true);  
        return commonResult;  
    }  
}
```

컨트롤러가 이걸 담아야만한다면 이런식으로 중복코드가 생성되고 있다.

이런 좋지않은 결과가 나오지않으려면, 

이런 메시지를 가공해주는 ==서비스==가 필요하다.

응답에 관한걸 제어해주는 팀이 필요하다는 말이다.


그래서 필요한 것이 바로  ResponseService다. 




## `service/ResponseService`

db필요없으니까 repository는 필요없다

```java
package com.example.baseframe.service;  
  
import com.example.baseframe.enums.ResultCode;  
import com.example.baseframe.model.CommonResult;  
import org.springframework.stereotype.Service;  
  
@Service  
public class ResponseService {  
    public CommonResult getSuccessResult() {  
        CommonResult result = new CommonResult();  
        result.setIsSuccess(true);  
        result.setCode(ResultCode.SUCCESS.getCode());  
        result.setMsg(ResultCode.SUCCESS.getMsg());  
          
        //값이 다 채워진걸 내뱉어 준다.  
        return result;  
    }  
}
```

와... 이렇게 하면 엄청 깔끔해지네

```java
package com.example.baseframe.controller;  
  
import com.example.baseframe.enums.ResultCode;  
import com.example.baseframe.model.CommonResult;  
import com.example.baseframe.service.ResponseService;  
import lombok.RequiredArgsConstructor;  
import org.springframework.web.bind.annotation.PostMapping;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
@RequiredArgsConstructor  
@RestController  
@RequestMapping("/test")  
public class TestController {  
      
    private final ResponseService responseService;  
      
    @PostMapping("/new")  
    public String memberJoin(){  
        return responseService.getSuccessResult();  
    }  
  
    @PostMapping("/admin/new")  
    public CommonResult setMember(){  
        CommonResult commonResult = new CommonResult();  
        commonResult.setCode(ResultCode.SUCCESS.getCode());  
        commonResult.setMsg(ResultCode.SUCCESS.getMsg());  
        commonResult.setIsSuccess(true);  
        return commonResult;  
    }  
}
```

정말 깔끔해졌네. 그런데 문제는 여기말고도 다른 컨트롤러도 쓸 수 있겠네.

그러니까 이 서비스는 그떄그때 부르지말고 항상 상시대기조로 쓸 수 있도록 static으로 올려두는게 좋겠다

메모리영역에서 정적영역으로 올려두란 말이야.



## static

```java
//ResponseService
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
}
```

```java
//Controller
@RequiredArgsConstructor  
@RestController  
@RequestMapping("/test")  
public class TestController {  
  
    @PostMapping("/new")  
    public CommonResult memberJoin(){  
        return ResponseService.getSuccessResult();   // 대문자다
    }  
  
    @PostMapping("/admin/new")  
    public CommonResult setMember(){  
        return ResponseService.getSuccessResult();  
    }  
}
```


앞으로는 ResponseService안에 들어가는 것,

- 성공했을 때 메시지 채울아이
- 실패했을 때 메시지 채울아이
- listResult 채울아이
- singleResult 채울 아이

이런것들을 만들어준다.