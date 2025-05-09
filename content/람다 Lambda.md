# stream
stream은 흘리는거다.
기존의 for문을 혼자 일을 처리하는거지만 stream은 막 흘려내는거다
흘리는걸 받아서 누군가가 처리해주는거다 = 병렬처리를하겠다

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





