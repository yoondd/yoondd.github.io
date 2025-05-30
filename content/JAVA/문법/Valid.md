
## `@Valid`란?

`@Valid`는 Java에서 **Bean Validation**(자바 표준 데이터 유효성 검사) 기능을 사용할 때 붙이는 어노테이션이다. 컨트롤러의 파라미터, 즉 요청(Request) 데이터를 검증할 때 많이 사용한다.

---

## 주요 역할

- **요청 데이터의 유효성(Validation) 검사**  
    클라이언트가 보낸 데이터가 지정한 조건(예: NotNull, Size 등)에 맞는지 자동으로 검사한다.
    
- **검증 실패 시 예외 발생**  
    조건을 만족하지 않으면 `MethodArgumentNotValidException` 등의 예외가 발생하고, Spring이 이를 잡아서 400 Bad Request 등의 에러를 반환한다.
    

---

## 사용 예시

## 1. 모델 클래스에 유효성 조건 추가

```java
public class PointRequest {
    @NotNull
    private Long memberId;

    @Min(1)
    private int point;

    // getters, setters
}
```

## 2. 컨트롤러에서 `@Valid` 사용

```java
@PostMapping("/put-point")
public String putPoint(@RequestBody @Valid PointRequest request) {
    memberPointService.putPoint(request);
    return ResponseService.getSuccessResult();
}

```

- `@RequestBody` : JSON 등으로 들어온 요청 데이터를 자바 객체로 변환
    
- `@Valid` : 변환된 객체의 필드에 붙은 유효성 어노테이션을 검사
    

---

## 자주 쓰는 유효성 어노테이션

- `@NotNull` : null이 아니어야 함
    
- `@NotBlank` : null, 빈 문자열, 공백 불가 (문자열)
    
- `@Size(min=, max=)` : 길이 제한
    
- `@Min`, `@Max` : 숫자 최소/최대값
    
- `@Email` : 이메일 형식
    

---

## 정리

- `@Valid`는 **요청 데이터의 유효성 검사**를 자동으로 해주는 어노테이션.
    
- 주로 모델 클래스의 필드에 유효성 어노테이션을 붙이고, 컨트롤러에서 `@Valid`를 사용해 검증
    
- 검증 실패 시 Spring이 자동으로 예외처리.



---



선생님의 설명. 

# Validation 

검증. 확인이라는 뜻을 가졌다.



프론트에서 1차적으로 validation을 하는 이유는? - 아무것도 안넣고 백한테 계속 요청을 보낸다면 너무 불편하잖아. 

서버 자원의 낭비라고도 볼 수 있다

서버, 백엔드 보호하기위해 간단하게라도 validation을 프로트에서 진행하기는 한다.



> 어떤 사용자가 pc와 모바일을 동시에 켜두고 가입을 진행한다고 보자. 프론트에서는 이미 둘다 검증되었으므로 통과할지라도, 백에서 service에서 튕기게 된다. throw가 일어나고 중복으로 입력되지않도록 처리할 수 있다. 


프론트가 - valid를 이용해서 null인지의 여부, 형식에 맞는지... 이런걸 검사했다.

중복되었는지.. 잘입력되었는지를 확인하기위해서는 ==service==에서 판단한다.

결국 이 패키지는 service에서 확인하는거다. 



> 경찰서에 면허증 갱신해야할 때, 직원에게 증명사진을 내는데 아이유사진을 넣으면 직원이 1차적으로 validation을 한다. 근데 정상적으로 냈을때는 직원이 내가 필요한 여부를 모르기때문에 일단 받고 서비스에 넘어가면 거기서 반려친다. 




### 사용하기

```java
@RequestBody @Valid MemberRequest request
```

위와같이 적으면된다.


대변검사를 할때, valid를 안달면 학교 선생님이 똥이 없어도 통과~시킨다. 

valid를 달면 봉투만 확인을 한다.

봉투 안에 있는 건 확인을 못한다



그러면 어떡하지? 안에든게 진짜 똥인지 아닌지 검사를하자

```java
class MemberRequest {

	@덩어리가섞였니
	@냄새나니
	private String ddong;
	
}
```

이런식으로 검사규칙을 정한다. 

봉투를 열어서 진짜 똥인지 확인을 하기위해서 체크를 한다. - 컨트롤러에서말이다.


누가 된장을 넣어갔어도, vaild만 있으면 통과하는데 - 실제로 MemberRequest에서 확인을 해야한다. 


---



이제 진짜 사람똥인지 강아지똥인지는 누가 검사해?
이제 서비스가 검사해야지. 

