샘플 프로젝트하면서 사용한 어노테이션을 모아봤다.

|**어노테이션 이름**|**설명**|
|---|---|
|@Override|메서드 오버라이드를 명시|
|@AllArgsConstructor|생성자|
|@NoArgsConstructor|디폴트생성자(파라미터가 없는 생성자) 만들어줌|
|@ToString|Override되는 toString메서드|
|@Controller|컨트롤러 생성|
|@Slf4j|[log.info](http://log.info)()사용을 위함|
|@GetMapping(”/”)||
|@PostMapping(”/’)|라우팅|
|@Autowired|스프링 부트가 미리 생성해둔 객체를 자동으로 연결해준다.|
|@Autowired||
|private ArticleRepository articleRepository;||
|@Entity|db가 해당 객체를 인식할 수 있도록 Entity 정의|
|@Column|db의 열 하나씩 정의함|
|@Id|primary key 지정|
|@GeneratedValue|하나씩 자동으로 수가 늘어나게끔 지정|
|@PathVariable|이변수는 path로 부터 가져온 변수다라는걸 알려주기위함|
|@RequestBody|body에서 찾아와라.|
|@Transactional|해당 메서드를 트랜젝션으로 묶어준다. 따라서 중간에 수행되지못한게 있으면 자동으로 rollback을 시켜서 처음으로 돌려준다. (트랜잭션이란 그런아이니까)|