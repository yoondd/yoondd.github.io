
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
    