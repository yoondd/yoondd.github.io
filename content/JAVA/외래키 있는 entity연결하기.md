service쪽 기능을 해야하는 것은 맞지만, 결합도가 너무 높아지면안되니까 서로 얽히지 않도록한다. 
실제 기능은 인포(컨트롤러)쪽에서 진행하도록 한다

```java
@RestController  
@RequiredArgsConstructor  
@RequestMapping("/jigak")  
public class JigakController {  
    private final MemberService memberService;  
    private final JigakHistoryService jigakHistoryService;  
  
    @PostMapping("/member-id/{memberId}")  
    public String setHistory(@PathVariable long memberId){  
        Member member = memberService.getData(memberId);  
        jigakHistoryService.setHistory(member);  
  
        return "OK!";  
    }  
}
```

위와같이 컨트롤러 내부에서 memberService와 jigakHistoryService를 불러온다. 


```java
@Service  
@RequiredArgsConstructor  
public class MemberService {  
    private final MemberRepository memberRepository;  
  
    //id 받아 서  찾은 거 내주 는  메서 드  
    public Member getData(long id){  
        return memberRepository.findById(id).orElseThrow();  
    }  
}
```

위 내용이 memberService인데 여기서는 id를 가져와서 Member를 내보내주는 메서드가 필요하다

```java
@Service  
@RequiredArgsConstructor  
public class JigakHistoryService {  
  
    private final JigakHistoryRepository jigakHistoryRepository;  
  
    public void setHistory(Member member){  
        JigakHistory jigakHistory = new JigakHistory();  
        jigakHistory.setMember(member);  
        jigakHistory.setDateJigak(LocalDate.now());  
        jigakHistoryRepository.save(jigakHistory);  
    }   
}
```

여기가 바로 fk를 가지고있는 jigakhistoryservice인데 여기서는 member를 가지고와서 저장한다.
여기서는 memberRepository를 가져오는 방식으로 코딩하지않는다.