
숫자로만들면 서로 알아듣기가 편하다


## BaseFrame 생성

다른 프로젝트의 베이스가 될 깨끗한 프로젝트를 생성한다




## 에러코드 enum

에러코드부터 만들어야한다 - enum

#### 변수를 숫자로 시작해선 안된다
그냥 리터럴과 헷갈릴 수가 있기때문에 상수든 변수든 숫자로는 절대 시작할 수 없다.

즉, 200 코드를 만들더라도 200으로 하지말라는 것이다. 무조건 영어로해라.

에러유형을 전부 넣는다, 문자열로 등록하는것이 중요하다.

-1000번대, -2000번대 등을 내가 구역잡아서 에러를 작성하는 것이다

```java
package enums;  
  
import lombok.Getter;  
import lombok.RequiredArgsConstructor;  
  
@RequiredArgsConstructor  
@Getter  
public enum ResultCode {  
  
    SUCCESS(0, "성공하였습니다")  
    , FAILED(-1, "실패했습니다")  
  
    , WRONG_PHONE_NUMBER(-1000, "잘못된 핸드폰 번호입니다")  
    , WRONG_ID(-1001, "잘못된 아이디입니다.")  
  
    , MISSING_DATA(-2000, "데이터가 없습니다")  
    ;  
  
    private final Integer code;  
    private final String msg;  
}
```

여기서 공통 인터페이스에다가 알맞게 넣어주는 것이다.  (code, msg)




## Exception

이건 그야말로 exit이다. 비상탈출구를 말하는 것이다.

위에서 만든 에러 상황에 맞게 각각의 비상출구를 제작해주어야한다는 말이다

성공은 탈출할 필요가 없으므로, 나머지만 만들어주면 되는데

일반적인 failed는 기본탈출구이다. 따라서, 얘는 탈출구를 만들어줄 필요가 없다.  - 기본탈출구처리

그럼 위 상황에서 몇개의 비상탈출구가 필요할까?

결국 수동으로 만들어줘야하는건 성공빼고, 기본 빼고 ==3개==다



#### 이름 짓기

나만의 비상탈출구를 지을 때에는 C를 붙이자.

custom탈출구의 약자다

`CWrongPhoneNumberException` 으로 짓자



#### 비상 탈출구의 실행시점

문제가 발생했을 때, 그때 실행이 되는거니까 .

런타임 시점에 실행이 되는거다


```java
package exception;  
  
public class CMissingDataException extends RuntimeException {  
}
```

RuntimeExceotion은 4개의 생성자가 필요하다

==오버로딩==을 이용해서 생성자를 여러가지 매개변수로 달리해서 사용해보자



#### CWrongPhoneNumberException

```java
package exception;  
  
public class CMissingDataException extends RuntimeException {  
  
    //아무런 매개변수가 없다면,  
    public CMissingDataException() {  
        super();  
    }  
  
    // 얘를 만들때 메시지를 받으면,  
    public CMissingDataException(String msg) {  
        super(msg);  
    }  
  
    public CMissingDataException(String msg, Throwable t) {  
        super(msg, t);  
    }  
  
}
```




## 결론

그러니까 enum과 exception은 짝꿍이라고 볼 수 있다.

에러코드가 겹치지 않게 하나를 새로 만들어주고, 출구도 만든다.


즉, 내 프로젝트에서 문제가 될만한 상황들을 모두 체크해서 

많이 쓸만한 기능들을 정리한 후, 전부 enum에서 만들고  exception으로도 만들어야한다

enum을 만들 땐, 특히 구역을 잘 짜서 나눠둬야지만 협업이 편하다

