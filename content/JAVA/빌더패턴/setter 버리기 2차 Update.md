이전에 이미 공부한 내용과 이어지는 강의다 [[setter 버리기 1차 Create]]

수정의 종류는 2개다 - 전체수정(put), 부분수정(patch)이다. 

부분수정도 엄청 많다

전체는 그저 하나다.



전체 수정이든 부분 수정이든 본질은 "수정".

그러니까 setter를 완전히 조져버리자. setter때문에 우리가 조짐을 당하고있다..ㅠ



## 1. 뭐에대한 수정인지를 먼저 정한다

멤버테이블의 주소수정이라는 주제 선정.

그러니까 도로명주소, 상세주소 받아서 수정하면 된다는 말이다.


```java
//service
public void putMemberByAddress(){

}
```

주소정보를 수정하겠다는 controller를 일단 만들었다.


잠깐, 모델필요하네?

```java
//MemberAddressUpdateRequest

@Getter
@Setter
public class MemberAddressUpdateRequest {
	private String address;
	private String detailAddress;
}
```

#### 여기 setter가 필요할까?  

사람이 넣을 수가 없는 상태가 되니까 그래도 있어야해.

값을 받아주는게 setter라서 어쩔 수가 없어

setter가 없으면 고객이 컨트롤러로 값을 입력하는것도 안되나보다.

그리고 builder패턴은 우리가 직접 하는거고 말이야(개발자)



#### 모든게 필수는 아니잖아?

`@NotNull` 으로 검사를 해달라고 한다

*검색어: spring boot validator 으로 검색해서 유효성검사할 수 있는거 어떤종류있는지 알아서 알아봐라*


```java
//MemberAddressUpdateRequest

@Getter
@Setter
public class MemberAddressUpdateRequest {

	@NotNull
	@Length(min=10, max=30)
	private String address;

	@NotBNull
	@Length(min=2, max=50)
	private String detailAddress;
}
```

request에서는 ==기본생성자==를 그냥 사용해야한다.



자, 다시 서비스로 넘어가보자.


```java
//service
public void putMemberByAddress(long id, MemberAddressUpdateRequest memberRequest){
	Member member = memberRepository.findById(id).orElseThrow();
	member.setAddress(updateRequest.getAddress());
	member.setDetailAddress(updateRequest.getDetailAddress());
	memberRepository.save(member);	
}
```

이게 기존 방식이거든? 

근데 이거 너무 짜증나. 왜냐면 setter가 있거든.

이거 수정할 줄 몰라서 지금 setter를 가지고 있거든?

이거 짜증나서 안되겠어.. 이거 모델 혹은 엔티티 안으로 이동하자 안되겠다.



빌더는 CRUD중에 C랑 U만 연관이 되어있다

빌더는 본질자체가 ==채워준다==거든.

쌓는건 사실 수정이 아니거든?

따라서 빌더는 C. 즉, 생성에서만 쓰이는거야.

그런데 빌더를 쓰려면 제약조건이, 바깥에서 newnew거리지 마세요. 이거거든.

그러니까 update도 빌더를 이용해야해. 



## 2. 수정에 관한 코드도 Member 엔티티 내에 넣자

빌더 내에 넣는것은 아니야.

```java
//Member

public void putAddress(MemberAddressUpdateRequest updateRequest){
	this.address = updateRequest.getAddress();
	this.detailAddresss = updateRequest.getDetailAddress();
}

```

자, 이것봐바.

일단 메서드 이름이 putAddress인 이유는 여기 자체가 member안에 있으니 member를 넣을 필요도 없기때문이야.

그리고 그 내부에 set..set..이런걸 적지않은이유는 위에 멤버변수같은걸로 있잖아.

결국 그 값을 변경하는거잖아.

왜 리턴이 없느냐 질문할 수도 있지만, 리턴은 필요없어

그냥 ***메모리에 올라간 값을 수정해내면 그만인걸***

```java
//service
public void putMemberByAddress(long id, MemberAddressUpdateRequest memberRequest){
	Member member = memberRepository.findById(id).orElseThrow();
	member.putAddress(updateRequest);
	memberRepository.save(member);	
}

```

자, 여기까지하면 세터가 완전히 제거가 되는거야.

놀랍지.



## U가 더 많아지면?

괜찮아. member엔티티에 적어주면될것을 뭘 고민을 해.

그냥 추가하면되잖아.


여기까지해서 setter가 사라지고 create, update기능까지 모두 할 수 있어졌어. 진짜 놀랍죠?

!!!



어렵게 생각하지마.

set이 builder로 넘어갔을 뿐이야.

진짜 간단한 내용이니까 흐름을 제대로 파악하자구.



---

# 도움받아 깔끔하게 정리하기

setter를 버리고 엔티티에 수정 책임을 주는 과정

## 엔티티 수정 책임 이전

### 1단계. 수정할 데이터를 담을 모델 만들기

- 수정에 필요한 값만 담는 모델을 만든다.
- 유효성 검사를 위한 어노테이션도 붙인다.


```java
@Getter 
public class MemberAddressUpdateRequest {     
@NotNull    
@Length
(min = 10, max = 30)    
private String address;     

@NotBlank    
@Length(min = 2, max = 50)    
private String detailAddress; 
}`
```

---


## 2단계. 컨트롤러에서 모델로 값 받기

- 클라이언트에서 넘어온 값을 모델로 받는다.
- @Valid로 유효성 검사도 같이 합니다.

```java
@PostMapping("/members/{id}/address") 
public ResponseEntity<Void> updateAddress(     
	@PathVariable Long id,    
	@Valid @RequestBody MemberAddressUpdateRequest request 
) {     
	memberService.putMemberByAddress(id, request);    
	return ResponseEntity.ok().build(); 
}
```

---

## 3단계. 서비스에서 엔티티 조회

- id로 엔티티를 DB에서 꺼낸다.

```java
public void putMemberByAddress(long id, MemberAddressUpdateRequest request) {     
	Member member = memberRepository.findById(id).orElseThrow();    
	// 다음 단계에서 엔티티의 수정 메서드 호출 
}
```


---

## 4단계. 엔티티의 수정 메서드 호출

- 엔티티 내부에 만든 수정 메서드를 호출해서 값 변경

```java
// Member 엔티티
public void putAddress(MemberAddressUpdateRequest request) {
    this.address = request.getAddress();
    this.detailAddress = request.getDetailAddress();
}

```

```java
// 서비스에서
member.putAddress(request);

```


---

## 5단계. 엔티티 저장

- 변경된 엔티티를 저장한다

```java
memberRepository.save(member);
```

---

## 전체 순서 요약

1. **DTO 생성**  
    (수정할 값 + 유효성 검사)
    
2. **컨트롤러에서 DTO로 값 받기**  
    (@Valid로 자동 검증)
    
3. **서비스에서 엔티티 조회**
    
4. **엔티티의 수정 메서드 호출**  
    (setter 대신 엔티티 메서드)
    
5. **엔티티 저장**
    

---

이렇게 하면 setter 없이도 안전하고 명확하게 값을 수정할 수 있다.

와우~