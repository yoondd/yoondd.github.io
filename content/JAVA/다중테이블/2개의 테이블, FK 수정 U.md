1번데이터 수정할건데요 -> jigakHistory에 pk가 1번인 데이터
(저 지금 2번인데... 이런말필요없이) 회원1번으로 수정해주세요 -> memberId 를 수정하는구나


patch는 body를 사용가능하다.

path + body
body + body



> 사실 FK가 아닌 다른 데이터를 수정하는 것은 쉽다. 어려운건 FK걸려있는 데이터를 수정하는 것. 여기서 어떻게 해야할지를 잘 머리속에 정리해두자. 
> 
> 아, 그런데 오해는 하지말자. DB관리 측면에서 실제로 우리가 FK를 건드릴 일은 사실 잘 없다. 

---

### jigakHistory테이블에 1번데이터가 1번이었는데 2번으로 수정하기.


model만들기

```java
package org.example.jigakpricegiveme.model;  
  
import lombok.Getter;  
import lombok.Setter;  
  
@Getter  
@Setter  
public class JigakMemberUpdateRequest {  
    private Long jigakHistoryId;  
    private Long memberId;  
}
```


service 수정하기

```java
public void putJigakMember(long id, Member member){  
    JigakHistory jigakHistory = jigakHistoryRepository.findById(id).orElseThrow();  
    jigakHistory.setMember(member);  
    jigakHistoryRepository.save(jigakHistory);  
}
```


controller 수정하기

```java
@PatchMapping("/member")  
public String putJigakMember(@RequestBody JigakMemberUpdateRequest request){  
    Member member = memberService.getData(request.getMemberId());  
    jigakHistoryService.putJigakMember(request.getJigakHistoryId(), member);  
    return "jigak member update!";  
}
```





#복수테이블CRUD