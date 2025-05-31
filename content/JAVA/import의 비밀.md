import는 수입을 말한다. 

다른 곳에 있는 파일 내용을 들여와서 쓸 수 있도록 만든다


```java
package com.example.baseframe.service;  
  
import com.example.baseframe.enums.ResultCode;  
import com.example.baseframe.model.CommonResult;  
import org.springframework.stereotype.Service;
```

엔터로 구분이 되어있는데 각각을 알아보자.


## package

우리가 친 적은 없다. 자동으로 들어가는 거다.

우리가 프로젝트를 만들 때, package name으로 자동으로 들어가는 부분이다.

전세계에서 유일한 이름 + service(폴더)

이 파일의 소스코드가 속해있는 폴더가 - 이 프로젝트의 service라는 폴더에요!

이렇게 자동으로 들어가는 것이다. 

모든 파일에 자동으로 다 들어간다. 

그럼 왜 맨 앞에 이걸 선언할까? -- 컴파일 할 때, 빠르게 찾기위함이다. 맨 윗줄만 읽으면서 기억을 딱 하는거다.

파일별로 찾을 수 있게 표를 싹 만들어둔거라고 보면 된다. 



## import

파일별로 잘 정리된 것중에 필요한 것을 불러오는 아이다.

여러개를 한번에 불러오려면 `*`을 붙여서 사용할 수도 있다.

표로 정리해두니까 잘 찾아갈 수 있다는 거다.


#### import 에러?

소스 코드 문제가 아니라 인텔리제이의 문제야.

그냥 도메인을 찾아서 가져오면되는데, 인텔리제이 자체에서 인식을 못하는거야

(파일 내용 복사해서 붙여넣을때 조심해. 패키지 이름이 달라서 아예 인식을 못할 수도 있어. )

그러니까 차라리 파일 자체를 통으로 복사해.

`컨트롤 + 쉬프트 + R` 한번에 replace도 하고.

이렇게 하면 파일 내용을 한번에 변경하는 유용한 단축키야.
