Java의 `repeat()` 메서드는 문자열을 지정한 횟수만큼 반복해서 
새로운 문자열로 만들어 반환하는 기능이다.
Java 11부터 `String` 클래스에 추가되었다고 한다. 


## 어떻게 사용하나?

`"문자열".repeat(n)` 방식으로 선언하면된다.
만약에 `n`이 0이면 빈 문자열을 반환하고, n이 1이면 원래 문자열을 반환한다.
혹시 `n`이 음수일 경우 `IllegalArgumentException` 예외가 발생한다고 한다.

```java
String str = "Hello";
System.out.println(str.repeat(3));  // HelloHelloHello
```


내부적으로 효율적인 복사 방식을 사용해 새 문자열을 만드는데, 기존에 for문이나 `StringBuilder`를 사용해 문자열을 반복하던 방식을 간단하게 대체할 수 있어 가독성과 성능 면에서 꿀이라고 한다
