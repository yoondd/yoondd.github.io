Swagger가 뭐냐면, Java에서 REST API 만들 때 진짜 많이 쓰는 오픈소스 도구야. API 명세를 자동으로 만들어주고, 문서화도 해주고, 심지어 웹에서 바로 테스트까지 할 수 있게 해줘. 개발자들끼리 API 스펙 공유할 때도 엄청 편하지.

---

## Swagger가 뭔지 한눈에 보기

- REST API 문서를 자동으로 만들어줘.
- 웹에서 API를 바로 테스트할 수 있어.
- 파라미터, 응답값, 에러 코드 같은 것도 다 문서에 포함시킬 수 있어.
- Java(Spring Boot)뿐만 아니라 여러 언어에서 쓸 수 있어.

---

## Swagger 구성 요소

- **Swagger UI**: 웹에서 API 명세를 예쁘게 보여주고, 직접 테스트도 할 수 있게 해주는 툴이야.
- **Swagger Editor**: 직접 YAML이나 JSON으로 API 명세를 작성하고, 실시간으로 확인할 수 있어.
- **Swagger Codegen**: 명세 파일로부터 서버 코드나 클라이언트 코드를 자동으로 만들어줘.

---

## Java(Spring Boot)에서 Swagger 적용하는 방법

1. **라이브러리 추가**  
    최근엔 `springdoc-openapi`를 많이 써.  
    예시로, build.gradle에 이렇게 추가하면 돼.
    
    `implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.1.0'`
    
    그리고 서버 켜면 `/swagger-ui/index.html`에서 바로 문서를 볼 수 있어.
    
2. **어노테이션으로 문서화**  
    컨트롤러나 메서드에 어노테이션만 달아주면 돼.
    이런 식으로 달아주면 Swagger UI에서 설명이 자동으로 보여.

```java
@RestController
@RequestMapping("/api")
@Tag(name = "인사", description = "인사 관련 API")
public class HelloController {
    @Operation(summary = "인사하기", description = "Hello 메시지 반환")
    @GetMapping("/hello")
    public String hello() {
        return "Hello, Swagger!";
    }
}

```

---

## Swagger의 장점

- API 명세가 항상 최신 상태로 유지돼.
- 프론트, 백엔드, 기획자 누구나 쉽게 API 구조를 이해할 수 있어.
- 문서화와 동시에 테스트까지 한 번에 가능!
- 협업할 때 커뮤니케이션이 훨씬 쉬워져.

