이넘들 중에 하나.

1. enums라는 폴더를 만들어둔다 - 대기장소
2. MemberGroup이라는 Enum클래스만들기

```java
public enum MemberGroup {
	ADMIN,
	USER
}
```

이러면 가명만보이네. 

```java
@Getter
@RequiredArgsConstructor
public enum MemberGroup{
	ADMIN("관리자", 10),
	USER("일반회원", 1),
	MANAGER("매니저", 5)

	private final String name;
	private final int level;
}
```

`ADMIN.name`하면 그 내부의 값을 가져올 수 있다
`ADMIN.level`이라고 하면 레벨값도 가져올 수 있다
실제 값은 ADMIN이지만 그 안에 비밀이 있다.
필드를 추가해서 쓰면된다.

*toList로 모든 내용을 긁어올 수도 있다.*


실무엔 ENUM이 진짜 많다.
일년에 얼마 안바뀌는 데이터는 그냥 ENUM으로 대체해서 사용하는 것이 훨씬 좋다.
