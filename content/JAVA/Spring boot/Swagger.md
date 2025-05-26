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




---



https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java

```java
// build.gradle
// https://mvnrepository.com/artifact/org.springdoc/springdoc-openapi-starter-webmvc-ui
implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.8.8'
```

```sh
//application.yml
springdoc:  
  api-docs:  
    enabled: true  # API 문서(`/v3/api-docs`) 활성화 여부 (기본값: true)  
    path: /v3/api-docs  # API 문서의 엔드포인트 변경 (기본값: /v3/api-docs)  
    groups.enabled: true  # 여러 개의 API 그룹을 지원할지 여부  
  
  swagger-ui:  
    path: /swagger-ui.html  # Swagger UI 접속 경로 변경 (기본값: /swagger-ui.html)  
    operationsSorter: method  # API 정렬 기준 (alpha: 알파벳순, method: HTTP 메서드 순)  
    tagsSorter: alpha  # 태그 정렬 방식 (alpha: 알파벳순 정렬)  
    display-request-duration: true  # 요청 실행 시간 표시 여부  
    doc-expansion: none  # Swagger UI에서 API 설명의 기본 펼침 상태 (none: 닫힘, list: 펼침, full: 전체 펼침)  
    persistAuthorization: true  # 페이지 새로고침 후에도 Authorization 헤더 유지 여부  
    defaultModelsExpandDepth: -1  # 모델 스키마의 기본 펼침 깊이 (-1이면 모델 펼쳐지지 않음)  
  
  paths-to-match:  # OpenAPI 문서에서 포함할 엔드포인트 패턴  
    - /v1/**  # /v1로 시작하는 모든 경로 포함  
    - /v2/**  # /v2로 시작하는 모든 경로 포함  
  cache:  
    disabled: true  # OpenAPI 문서의 캐싱 비활성화 (기본값: false)

```


