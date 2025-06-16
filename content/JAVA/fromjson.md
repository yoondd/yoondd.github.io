컴파일 언어 dart를 기준으로 강의해주셨당

response에는 fromjson이 있고,

request는 tojson이 있다

```dart
final body = jsonEncode({
	'name': 'ddd',
	'phoneNumber': '010-1111-2222'
})
```

이런식으로하면 컴파일언어인데 제대로 관리하기가 쉽지않다

컴파일언어의 특징에 맞게 무조건 model을 만들자

그래야 ==협업==이 가능하다.


```dart
class TestRequest{

}
```

클래스에는 매개변수 넣을 수 있는 자리가 없다.

틀인데 무슨 매개변수가 있겠어. 그냥 틀 자체를 언급하면되지.


## final사용하기

이제는 모델에 무얼 담을지를 보면된다 ( 사실 swagger보면서 해야하는데 지금은 없어서 그냥 한다 )

```dart
class TestRequest {
	final String name;
	final String phoneNumber;
}
```

처음에 담고 한번도 수정안할것은 final 으로 작성해라

final이 있으니까 생성자를  반드시 만들어주어야 한다

final이 없다면 기본생성자를 쓰면되는데 final이 있으니까 무조건 넣어줘야하거든


## 생성자만들기

```dart
class TestRequest {
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

프론트에게 웬만하면 null을 주지말자 


## factory를 왜 쓸까?

팩토리? 공장이네?
