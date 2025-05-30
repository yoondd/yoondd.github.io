다시 얘기하지만 서비스는 값채우라고 쓰는게 아니다.

값을 채워내는 반복작업은 다른애한테 시켜야한다.

누구한테 시킬 것인가.... 그얘기는 나중에 하고,

`Member member = new Member()` 이런식으로 저 빈 괄호가 등장하지 않도록 해야한다.

즉, setter를 만들 수 있는 상황을 아예 없애하는 이말이다.


## 1. `interfaces` 폴더 제작

interface는 살은 채워지지않은 그저 뼈대를 말한다.

살 채우는 소스코드는 진짜 중요한 내용을 말한다.

```java
// interfaces/CommonModelBuilder
public interface CommonModelBuilder<T> {
	T build();
}
```

이거 implement하게되면 반드시 build()를 구현해야한다


## 2. `entity/Member`

빈그릇을 만들지 않도록 처리해야한다. 

빈그릇 뿐 아니라 member라는 생성자를 쓰지못하도록 막아버려야한다.


#### (1) 기본생성자 막기

```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

}
```

같은 패키지 안에서만 쓸 수 있는 protected를 사용한다

서비스는 같은 패키지가 아니므로 못본다 (서비스에서 못쓰게 한다 )

자, No ~이거를 썼으니까 아예 생성자를 못쓴다는 소리다


이제는, 멤버를 만들려면 builder를 사용해서 builder가 값을 넣어주도록 만들어야한다

builder는 어디에있어야하냐? - 엔티티 클래스 내부에 들어가면된다.

클래스 안에 클래스 가능? - 쌉가능

틀 안에 당연히 틀이 들어갈 수 있으니까~


#### (2) 빌더 만들기

```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

	//,.. 앞쪽 생략
	
	public static class Builder implements CommonModelBuilder<Member>{
		@Override
		public Member build(){
			return null;
		}
	}

}
```

member를 위한 빌더를 만들겠다는 소리다

인터페이스가 있으면 alt+enter으로 바로 채워넣을수가 있다


여기서 방금 만든 클래스 내부의 클래스는 다른곳에서 접근하기가 쉽지않아.

따라서 ==static==으로 변경해야만 한다.



#### (3) 생성자만들기

빌더가 값을 받아서 이제 멤버에 넘겨줘야하는데,

이거는 빌더만 알아야한다는거야.

생성자를 private로 하나 새로 만들어줘여해


```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

	//,.. 앞쪽 생략

	private Member(Builder builder){
		
	} // 어? 리턴타입이 없어. 이거가 바로 생성자야.

	
	public static class Builder implements CommonModelBuilder<Member>{
		@Override
		public Member build(){
			return null;
		}
	}

}
```

리턴타입이 없는 생성자를 만들어준다

빌더를 받아서 만들어주는 애라는 소리다


자, 이제부터는 setter자체를 쓸 수가 없다.



#### (4) 빌더가 무얼 받아야하는지 작성해보자


```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

	//,.. 앞쪽 생략

	private Member(Builder builder){
		
	} // 어? 리턴타입이 없어. 이거가 바로 생성자야.

	
	public static class Builder implements CommonModelBuilder<Member>{

		private final String name;
		private final String phonenumber;
		...
		...
		....
		private final Boolean isLeave;

		@Override
		public Member build(){
			return null;
		}
	}

}
```

컨트롤 알트 쉬프트를 한번에 눌러서 한꺼번에 선택해서 가져와, 아주 편해

위에서 가져오라구.

여기다가 다 넣어놓고 final을 다 때려넣어.



#### (5) 서비스에서 부를 수 있는 메서드 구현

```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

	//,.. 앞쪽 생략

	private Member(Builder builder){
		
	} // 어? 리턴타입이 없어. 이거가 바로 생성자야.

	
	public static class Builder implements CommonModelBuilder<Member>{

		private final String name;
		private final String phonenumber;
		...
		...

		public Builder(MemberCreateRequest request){
			this.name = request.getName();
			this.username = request.getUsername();
			.
			.
			.
			.
			this.address = request.getAddress();
			this.joinDate = LocalDate.now();
			this.birthDay = LocalDate.of(request.getBirthYear(), reques...)
			..
			..
			this.isLeave = false;
		}
			

		@Override
		public Member build(){
			return new Member(this);
		}
	}

}
```

서비스에서 호출할 수 있는 빌더를 만들어준다. 

자, 무슨작업을 했냐면, builder라는 메서드를 만들어 내부를 채웠다.

this.의 내용물을 채우게 만들어줬다는 소리다.

그리고 나서 override로 만든 build라는 메서드의 return 값을 new Member(this)로 바꿔주었다



#### (6) 값을 옮겨준다

```java
@Entity
@Getter
@Setter
@NoArgsContructor(access = AccessLevel.PROTECTED)
public class Member{

	//앞쪽생략..
	@Column(nullable = false)
	private Boolean isLeave();
	//,.. 앞쪽 생략

	private Member(Builder builder){
		this.name = builder.name;
		this.username = builder.username;
		this.password = builder.password;
		this.email = builder.email;
		.
		.
		.
		this.isLeave = builder.isLeave;
	}

	


```

컨트롤 알트로 정리도 가능하다. 

대강 복사해서 쓰고 정리해라.

이렇게 생성자를 통해 안에 값을 입력해주는 거다.




## 3. `MemberService`

이제는 setter같은건 필요없다.

예쁘게 모양만드는부분을 수정하자


```java
public void setMember(MemberCreateRequest request){
	if(isDuplicateInfo(request.getUsername(), requ...)) //중복제거부분

	Member member = new Member.Builder(request).build();

	memberRepository.save(member);
}
```

세상에. 그 많던 setter가 사라진 것을 볼 수 있다.

이런 과정을 통해 setter를 없앴고, 그 모든 과정을 model에서 구현하도록 builder를 세팅했다