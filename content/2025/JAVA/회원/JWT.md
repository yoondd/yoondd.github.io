JWT를 사용하는 인증 워크플로우는 다음과 같이 진행됩니다.

---

## JWT 인증 워크플로우

1. **로그인 요청**
    - 사용자가 ID와 비밀번호(자격 증명)를 입력하여 서버에 로그인 요청을 보낸다.
    
2. **서버 인증 및 토큰 발급**
    - 서버는 입력받은 자격 증명을 검증한다.
    - 인증에 성공하면 서버는 JWT(액세스 토큰, 필요시 리프레시 토큰도 함께)를 생성하여 클라이언트에 반환한다. 이때 각각의 포트가 달라도 아무튼 JWT는 그냥 보내기만 하는거니까 상관이없다.
        
3. **클라이언트 토큰 저장**
    - 클라이언트는 받은 JWT를 로컬 스토리지, 세션 스토리지, 혹은 HttpOnly 쿠키 등에 저장한다.
    - 보안상 HttpOnly 쿠키 사용이 권장된다.
        
4. **API 요청 시 토큰 전송**
    - 이후 클라이언트는 보호된 API를 호출할 때마다 Authorization 헤더에 JWT를 담아 서버로 전송한다.
    - 예를 들면 이렇게 말이다.
        `Authorization: Bearer <JWT>`
        
5. **서버의 토큰 검증**
    - 서버는 요청을 받을 때 JWT의 유효성을 검증한다.(서명, 만료시간, 권한 등)
    - 유효하다면 인증된 사용자로 인식하여 요청을 처리한다.
        
6. **토큰 만료 시 갱신
    - 액세스 토큰이 만료된 경우, 클라이언트는 리프레시 토큰(별도 저장)을 이용해 새로운 액세스 토큰을 요청할 수 있다.
    - 리프레시 토큰도 만료되었거나 유효하지 않으면 다시 로그인한다.
        

---

## **시퀀스 다이어그램 요약**

```
사용자 → 서버: 로그인 요청 (ID/비밀번호)
서버 → 사용자: JWT(액세스 토큰 등) 발급
사용자: 토큰 저장
사용자 → 서버: API 요청 (Authorization: Bearer <JWT>)
서버: 토큰 검증 후 요청 처리
(만료 시) 사용자 → 서버: 리프레시 토큰으로 액세스 토큰 재발급 요청
```



---

# 실제로 쓰이는 로직을 볼까?


### 백엔드 - JWT 유틸리티 클래스 (토큰 생성)

```java
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import java.util.Date;

public class JwtUtil {
    private final String secretKey = "yourSecretKey"; // 환경변수로 관리 권장

    public String generateToken(String username, String role) {
        return Jwts.builder()
            .setSubject(username)
            .claim("role", role)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60)) // 1시간 유효
            .signWith(SignatureAlgorithm.HS256, secretKey.getBytes())
            .compact();
    }
}

```

아직.... 어떻게 토큰을 생성하는지에 대한 전반적인공부가 필요하다.



### 프론트엔드 - 토큰 저장

```tsx
// 로그인 후 응답에서 JWT 추출
const token = response.headers.get('Authorization').split(' ')[1];
// 쿠키 또는 localStorage 등에 저장
localStorage.setItem('jwt', token);

```


### 프론트엔드

```java
const token = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...'; // 발급받은 JWT

fetch('https://api.example.com/user/profile', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
});

```


### 백엔드

```java
String bearerToken = request.getHeader("Authorization");
if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
    String token = bearerToken.substring(7); // "Bearer " 이후의 실제 JWT만 추출
    // 토큰 검증 로직
}

```