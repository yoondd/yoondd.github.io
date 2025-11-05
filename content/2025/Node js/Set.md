말그대로 집합이다.
중북되지않은 유일한 값들의 집합을 저장하는데 사용된다.

# 특징이 뭐야?

1. 중복허용을 안한다 : 아무리 같은 값을 저장하려고해도 안된다!!
2. 순서를 보장한다 - 이터러블
3. 모든 데이터타입을 허용한다
4. 빠른조회와 삭제가 가능하다


# 어떻게 만들어?

```js
const mySet = new Set();
```


# 중복제거를 어떻게 하지?

```js
const mySet = new Set([1, 2, 3, 3, 4]);
console.log(mySet); // Set { 1, 2, 3, 4 } (중복 제거)
```


# 어떤 메서드가 있는데?

| 메서드                 | 설명                                        |
| ------------------- | ----------------------------------------- |
| `add(value)`        | 값을 추가하고 `Set` 객체를 반환합니다.                  |
| `delete(value)`     | 특정 값을 제거하고 성공 여부를 반환합니다 (`true`/`false`). |
| `has(value)`        | 특정 값이 존재하는지 확인하여 불리언 값을 반환합니다.            |
| `clear()`           | 모든 값을 제거합니다.                              |
| `size`              | Set에 저장된 값의 개수를 반환하는 속성입니다.               |
| `forEach(callback)` | 각 요소에 대해 콜백 함수를 실행합니다.                    |
| `values()`          | 모든 값을 포함하는 반복자(iterator)를 반환합니다.          |
|                     |                                           |



# 예제 좀 살펴볼까?


```js
const mySet = new Set();
mySet.add(1);
mySet.add(2);
mySet.add(2); // 중복된 값은 무시됨
mySet.add("hello");
console.log(mySet); // Set { 1, 2, 'hello' }

```



# 활용하기 - 배열의 중복값 제거하기

```js
const numbers = [1, 2, 3, 4, 4, 5, 5];
const uniqueNumbers = [...new Set(numbers)];
console.log(uniqueNumbers); // 출력: [1, 2, 3, 4, 5]
```

아하, set에 한번 돌리면 이녀석은 중복이 싹 다 제거되는구나!!


# 활용하기 - 교집합, 차집합 찾아내기

```js
const setA = new Set([1, 2, 3]);
const setB = new Set([3, 4, 5]);

// 교집합 (Intersection)
const intersection = new Set([...setA].filter((x) => setB.has(x)));
console.log(intersection); // 출력: Set {3}

// 차집합 (Difference)
const difference = new Set([...setA].filter((x) => !setB.has(x)));
console.log(difference); // 출력: Set {1, 2}
```
