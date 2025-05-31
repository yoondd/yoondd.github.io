
## 자바 스트림(Stream) 완전 간단 정리

**스트림이 뭐냐?**  
자바 8부터 나온 기능인데, 리스트나 배열 같은 데이터들을 반복문 없이 한 줄로 쭉쭉 가공할 수 있게 해주는 멋진 도구야. 필터링, 변환, 합계 이런 거 다 한 방에 처리 가능!

---

### 스트림 기본 구조

```java
컬렉션.stream()
    .중개연산1()
    .중개연산2()
    ...
    .최종연산();
```
- **중개연산:** filter, map, sorted 등 (중간에 데이터 가공)
- **최종연산:** forEach, collect, count 등 (마지막에 결과 뽑아냄)

---

### 스트림 만드는 방법

- 리스트: `list.stream()`
- 배열: `Arrays.stream(array)`
- 직접 값 넣기: `Stream.of(값1, 값2, ...)`

---

### 자주 쓰는 예시

```java
List list = Arrays.asList("a", "b", "c");

// 'b'만 골라서 출력
list.stream()
    .filter(s -> s.equals("b"))
    .forEach(System.out::println);

// 전부 대문자로 바꿔서 리스트로 만들기
List upper = list.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

// 길이가 1 넘는 애들 개수 세기
long count = list.stream()
    .filter(s -> s.length() > 1)
    .count();
```

---

### 스트림 쓰면 좋은 점

- for문 안 써도 돼서 코드가 짧고 깔끔해짐
- 함수형 스타일이라 가독성 굿!
- 병렬 처리도 쉽게 할 수 있음
