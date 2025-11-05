dart에서는 new키워드를 쓰지 않는다

공장처럼 빵빵 찍어내는아이라고 생각하면된다

factory를 넣으면 생성자를 만드는데 이름을 바꿔서 생성자를 만들 수 있다

결국에 생성자가 필요한데, 무조건 클래스 이름과 같은것으로만 만들고 매개변수만 달리하면 좀 가독성이 떨어지고 힘들더라고

그래서 생성자의 이름을 달리 사용할 수 있게 만드는것이 바로 factory다

유지보수가 훨씬 좋겠네 - 항상 유지보수에 신경쓰고 개발작업을 하자


factory가 달리면 이름이 바뀐 생성자다 라고 생각하면된다


```dart
class TestResopnse {
	final String name;
	final String phoneNumber;

	TestRequest(this.name, this.phoneNumber);

	factory TestResponse.fromJson(Map<String, dynamic> json){
		return TestResponse(
			json['id'],
			json['goodsName'],
			json['owner'],
			json['contents']
		) 
	}
}
```

### fromjson을 생성자를 이용해서 받은 이유가 무엇인가?

생성자의 장점은 무엇인가 - 

상단에 보니 final이다. 한번 넣으면 안바뀌게 해두었다는거네.

애초에 이 클래스 용도가 api호출하면 받아와서 담는거잖아

받아온걸 뭐 바꿀일이 있는가, 없잖아.

그래서 final이야. 원본자체는 보존을 해둬야하니까. 절대 고치면 안되니까 말이야.



## request해볼까?

이제는 tojson도 만들어야겠네.

```dart
class TestRequest {
	final String name;
	final String phoneNumber;

	TestRequest(this.name, this.phoneNumber);

}
```

### tojson은 팩토리로 만들어야하나?

흐름을 생각해보면 답이 나오지.

코드로 옮겨오는거잖아. 

request를 쓰는 페이지가 어디야? page_post페이지잖아.

거기서 `jsonEncode`를 사용해야하네. 

결국에 저 질문은, 생성자로 받아야하냐 아니냐 그 얘기야.

jsonEncoding- json으로 만드는거야. 역직렬화.를 말하는거야

생성자를 통해서 객체를 생성하게되면 - 걔는 json이 아니잖아. 객체가 되어버리잖아

그러니까 여기서는 팩토리를 쓰면안되지. 


##  json의 리턴타입?

맵이야.

```dart
class TestRequest {
	final String name;
	final String phoneNumber;

	TestRequest(this.name, this.phoneNumber);

	Map<int, dynamic> toJson() {
		
	}

}
```

어? 근데 여기 보니까, 이상한게 하나 있네. 

 `<int, dynamic>` 잠깐만, 여기에는 레퍼클래스가 들어가야하잖아.

왜 여기서 에러가 안뜨지?

왜 자바에서는 레퍼클래스만 썼었나? - 리터럴이니까.  (자바에서는 int, Integer이런식으로 구분이 되잖아)

근데 여기서는 그게아니네.



자, 다시 본론으로 넘어가서

```dart
class TestRequest {
	final String name;
	final String phoneNumber;

	TestRequest(this.name, this.phoneNumber);

	Map<String, dynamic> toJson() {
		'name': name,
		'phoneNumber': phoneNumber,
	}

}
```

근데 매개변수는 왜 없어?

TestRequest(this.name, this.phoneNumber)에서 이미 값 받아오니까 괜찮아

바로 그냥 return 해도돼 



자, 여기까지 했으면 이제 page_post에 가서 encode 부분을 변경하면돼

```dart
TestRequest request = TestRequest('김기기', '010-1111-1111');

...

final response = await http.post(
	..
	body: jsonEncode(request.toJson())
)
```


여기까지야.

내가 dart 를 잘 모르긴하지만 전체적인 느낌안 이해하고 넘어가자.