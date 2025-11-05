### 1. docker run

```sh
docker run --name present -e POSTGRES_USER=present -e POSTGRES_PASSWORD=present1234 -p 10005:5432 -d postgres:16
```
도커를 띄운 후, 데이터 그립에서 확인해봐도 좋고.


### 2. yml

```yml
server 
	port: 8888
```
포트가 겹치면 다른 포트를 입력해 주어도 좋다.


### 3. entity

```java
@Entity  
@Getter  
@Setter  
public class Present {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column(nullable = false)  
    private String name;  
}
```


### 4. repository

```java
public interface PresentRepository extends JpaRepository<Present, Long> {  
}
```


### 5. service

```java
@Service  
@RequiredArgsConstructor  
public class PresentService {  
  
    private final PresentRepository presentRepository;  
  
    public void setPresent(PresentCreateRequest request){  
        Present present = new Present();  
        present.setName(request.getName());  
        presentRepository.save(present);  
    }  
}
```


### 7. model

```java
@Getter  
@Setter  
public class PresentCreateRequest {  
    private String name;  
}
```


### 6. controller

```java
@RestController  
@RequiredArgsConstructor  
@RequestMapping("/present")  
public class PresentController {  
  
    private final PresentService presentService;  
  
    @PostMapping("/new")  
    public String setPresent(@RequestBody PresentCreateRequest request){  
        presentService.setPresent(request);  
        return "add present success";  
    }  
}
```


create까지 작성하는 방법을 적어봤다. 
오랜만에 하니까 헷갈린다;