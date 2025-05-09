# stream - 병렬처리
stream은 흘리는거다.
기존의 for문을 혼자 일을 처리하는거지만 stream은 막 흘려내는거다
흘리는걸 받아서 누군가가 처리해주는거다 = 병렬처리를하겠다

***병렬처리가 핵심이다***

자바스크립트에서 stream이 없다 왜냐면 싱글스레드이니까.

```java
public static void main(String[] args){
	List<Integer> n = Arrays.asList(1,2,3,4,5,6,7,8,9);
	n.stream() //흘리는건 한번이다
		.filter(i->i%2 ==1)
		.forEach(System.out::println)  //이게 무슨뜻이야
}
```

필터를 장착을 해서 걸러주는거다.  
`i -> i %2 ===1`  여기서 람다식이 나오는거다. 람다는 익명함수거든.
여기서 return은 오직 하나여야만한다. 
`->`은 리턴을 의미한다. 이게 기본법칙이다
그 앞은 *매개변수*자리, 그 뒤는 *처리할 코드부분*이다.

자바스크립트에서는 필터가 고차함수다. 


#### List타입으로 바꾼 이유?
array는 index기반이라서 stream접근이 안된다.

#### asList를 왜 썼을까?
add()해가지고 집어넣기 너무 불편해서..
그냥 배열은 일렬로 쓸 수 있으니까 얼마나 편해.. 결국 편할라고 한거야 ㅋㅋ

#### System.out::println - 메소드 참조(::)
이거 뭐지.. println이 실행되는건 알겠는데.
값이 안넘어가네?  - 이거를 아주 간소화한거야
`forEach(i->System.out.println(i))` 원래 이거거든.
이거를 더 짧게 줄일수가 있어. `System.out::println` 내가 뭐 줄테니까 그냥 실행해! 이러는거다
`println`을 바로 호출할건데 ::앞에 주인을 넣어주는거다. 
*그런데 매개변수가 여러개일때는 못쓴다.* 그냥 코드가 한줄일때만 쓸 수 있는거다

```java
forEach(i->System.out.println(i))
forEach(System.out::println)
```

이렇게 매개변수가 하나일때는 줄일수가 있는데, 매개변수가 여러개면 안된다



---

## map

```java
List<String> a = Arrays.asList("aaa","bbb", "cc");
a.stream()
	.map(i->i.toUpperCase())
	.forEach(System.out::println);

```

#### i->i.toUpperCase() 를 줄여볼까?
 ::이걸 쓰려면 어디소속인지 모르니까 일단 람다로 조진다
 쭉 올리면 string이라는 소속이 나온다

```java
map(String::toUpperCase)
```



---

### reduce

```java
List<Interger> nums = Arrays.asList(1,2,3,4,5,6,7);
int sum = nums.stream().reduce(0, (a,b)->a+b);

```

a가 0을 뜻하고, b는 흘려서 얻는 존재를 말한다.

```java
int sum = nums.stream()
			.filter(i->i%2==0)
			.reduce(0, (a,b)->a+b);
```

