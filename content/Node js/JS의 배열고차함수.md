# sort

sort는 배열의 요소를 적절하게 정렬한다. 
오름차순이나 내림차순으로 정렬할 수 있다. 
==원본파괴==하여 직접 배열을 건드린다.


```js
const fruits = ['Banana', 'Orange', 'Apple'];

// ascending(오름차순)
fruits.sort();
console.log(fruits); // [ 'Apple', 'Banana', 'Orange' ]

// descending(내림차순)
fruits.reverse();
console.log(fruits); // [ 'Orange', 'Banana', 'Apple' ]
```

오름차순이나 내림차순으로 정렬한다고 했지만, 이는 문자열 정렬이기때문에 유니코드 순서대로다.
숫자를 정렬할때는 다른 방식을 사용해야한다.

```js
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열 오름차순 정렬
// 비교 함수의 반환값이 0보다 작은 경우, a를 우선하여 정렬한다.
points.sort(function (a, b) { return a - b; });
console.log(points); // [ 1, 2, 5, 10, 25, 40, 100 ]

// 숫자 배열 내림차순 정렬
// 비교 함수의 반환값이 0보다 큰 경우, b를 우선하여 정렬한다.
points.sort(function (a, b) { return b - a; });
console.log(points); // [ 100, 40, 25, 10, 5, 2, 1 ]

```



# forEach

가장 기본이 되는 배열 고차함수로 for문 대신 사용할 수 있다.
배열 순회하면서 콜백함수를 실행한다. 
==원본은 건드리지않는다==
매개변수로는  value, index, array를 받는다.
for문과는 달리 break를 사용하지않는다. - 순회 중단이 불가하다
for문보다 성능이 좋은 것은 아니지만 가독성이 좋다.

```js
numbers.forEach(function (item, index, self) {
  console.log(`numbers[${index}] = ${item}`);
  total += item;
});
```



# map

배열 순회하는 것은 똑같으나 리턴해서 ==새로운 배열==을 완성해낸다. ==원본 배열을 하지않는다==
즉, 다른 값으로 매핑하기 위한 함수라고 보면 된다. (한바퀴 쭉 돌려서 배열을 만들어내고싶을때)
v, i, a를 받는다는 점에서 똑같다. 

```js
const numbers = [1, 4, 9];

const roots = numbers.map(function (item) {
  return Math.sqrt(item);
});

// 위 코드의 축약표현은 아래와 같다.
// const roots = numbers.map(Math.sqrt);

console.log(roots);   // [ 1, 2, 3 ]
console.log(numbers); // [ 1, 4, 9 ]
```



# filter

배열을 만들어낸다는 점에서  map과 비슷하지만 filter는  if문을 대체할 수 있다. 
배열을 순회하며 각 요소에 대하여 인자로 주어진 콜백함수가 "참"인 결과만을 뽑아내서 배열을 반환한다.

```js
const result = [1, 2, 3, 4, 5].filter(function (item, index, self) {
  console.log(`[${index}] = ${item}`);
  return item % 2; // 홀수만을 필터링한다 (1은 true로 평가된다)
});

console.log(result); // [ 1, 3, 5 ]
```




# reduce

배열을 순회하며 각 요소에 대하여 이전의 콜백함수 실행 반환값을 전달하여 콜백함수를 실행하고 그 결과를 반환한다. 

```js
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce(function (previousValue, currentValue, currentIndex, self) {
  console.log(previousValue + '+' + currentValue + '=' + (previousValue + currentValue));
  return previousValue + currentValue; // 결과는 다음 콜백의 첫번째 인자로 전달된다
});
```



# some 

배열 안에있는 요소가 콜백함수의 테스트를 통과하는지 확인해서 그 결과값을 boolean으로 반환한다.
하나라도 통과하면 true를 반환한다.

```js
// 배열 내 요소 중 10보다 큰 값이 1개 이상 존재하는지 확인
let res = [2, 5, 8, 1, 4].some(function (item) {
  return item > 10;
});
console.log(res); // false

res = [12, 5, 8, 1, 4].some(function (item) {
  return item > 10;
});
console.log(res); // true

// 배열 내 요소 중 특정 값이 1개 이상 존재하는지 확인
res = ['apple', 'banana', 'mango'].some(function (item) {
  return item === 'banana';
});
console.log(res); // true
```



# every

some 과 비슷한데 이번에는 모든 아이들이 다 통과해야한다. 

```js
// 배열 내 모든 요소가 10보다 큰 값인지 확인
let res = [21, 15, 89, 1, 44].every(function (item) {
  return item > 10;
});
console.log(res); // false

res = [21, 15, 89, 100, 44].every(function (item) {
  return item > 10;
});
console.log(res); // true
```




# find 

배열을 순회하면서 각 요소에 대해 인자로 주어진 콜백함수를 실행하고,
그 결과가 참인 첫번째 요소를 반환한다. 
혹시라도 참인것을 발견하지 못했을 때에는 undefined를 반환한다.
v, i, a를 가져갈 수 있다. 
filter와 비슷하지만, filter는 언제나 배열을 추출하는 반면에 find는 콜백함수를 실행하여 그 결과가 참인 ==첫번째 결과값을 반환==하므로 결과값이 언제나 일반 요소값이다.

```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

// 콜백함수를 실행하여 그 결과가 참인 첫번째 요소를 반환한다.
let result = users.find(function (item) {
  return item.id === 2;
});

// ES6
// const result = users.find(item => item.id === 2;);

// Array#find는 배열이 아니라 요소를 반환한다.
console.log(result); // { id: 2, name: 'Kim' }

// Array#filter는 콜백함수의 실행 결과가 true인 배열 요소의 값만을 추출한 새로운 배열을 반환한다.
result = users.filter(function (item) {
  return item.id === 2;
});

console.log(result); // [ { id: 2, name: 'Kim' },{ id: 2, name: 'Choi' } ]
```




# findIndex

마지막으로 알아볼 배열고차함수!!!
v,i,a를 받을 수 있고, 배열 순회하면서 콜백함수 실행하여 그 결과가 참인 첫번째 요소의 인덱스를 반환한다. 
콜백함수의 실행 결과가 참인 요소가 존재하지 않으면 -1을 반환한다.

```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

// 콜백함수를 실행하여 그 결과가 참인 첫번째 요소의 인덱스를 반환한다.
function predicate(key, value) {
  return function (item) {
    return item[key] === value;
  };
}

// id가 2인 요소의 인덱스
let index = users.findIndex(predicate('id', 2));
console.log(index); // 1

// name이 'Park'인 요소의 인덱스
index = users.findIndex(predicate('name', 'Park'));
console.log(index); // 3
```