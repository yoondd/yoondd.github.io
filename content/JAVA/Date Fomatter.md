
날짜 타입을 요청자에게 보내줄때, LocalDate형식으로 내보내려고하니까 출력타입이
`2024-03-11T11:11:11` 이런 형태다.
너무 불편해가지고 프론트를 위한 방식으로 **텍스트형태**로 만들어줘야한다.

```java
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("dd MMM uuuu");
```

위와같은 형태로 만들어야한다. 