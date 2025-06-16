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

}
```

### tojson은 팩토리로 만들어야하나?

흐름을 생각해보면 답이 나오지.

코드로 옮겨오는거잖아. 

request를 쓰는 페이지가 어디야? page_post페이지잖아.

거기서 `jsonEncode`를 사용해야하네. 

결국에 저 질문은, 생성자로 받아야하냐 아니냐 그 얘기야.

jsonEncoding- json으로 만드는거야. 역직렬화.를 말하는거야

