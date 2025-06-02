자바의 `toCharArray()` 메서드는 `String` 객체를 문자 배열(`char[]`)로 변환해주는 메서드다. 
쉽게 말해서 *문자열을 한 글자씩 쪼개서 각각을 배열 요소로 만들어 반환한다*

```java
String str = "HELLO";
char[] chars = str.toCharArray();

for (char c : chars) {
    System.out.println(c);
}
```

이 코드를 실행하면, 모두 조각조각 잘라서 표현할 수 있다.

```
H
E
L
L
O
```

- 반환되는 배열의 길이는 원래 문자열의 길이와 같으며, 공백도 하나의 문자다.
- 변환된 배열은 원본 문자열과 독립적이어서 배열을 수정해도 원본 문자열에는 영향을 주지 않는다.