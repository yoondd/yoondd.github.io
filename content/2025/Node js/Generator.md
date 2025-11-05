이터러블을 생성하는 함수.
제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 좀 더 간편하게 이터러블을 구현할 수 있고, ==비동기 처리에 유용==하게 사용된다.

```js
// 무한 이터러블을 생성하는 제너레이터 함수
function* createInfinityByGenerator() {
  let i = 0;
  while (true) { yield ++i; }
}

for (const n of createInfinityByGenerator()) {
  if (n > 5) break;
  console.log(n); // 1 2 3 4 5
}
```


# function*

function* 는 제너레이터 함수를 정의하기 위한 JavaScript의 특별한 문법. 
제너레이터 함수는 일반 함수와 달리 ==실행을 중간에 멈추고, 필요할 때 다시 재개할 수 있는 기능을 제공==한다 
이를 통해 값을 하나씩 반환하거나, 반복 가능한 데이터를 생성하는 데 유용하다.


# yield

`yield`는 JavaScript에서 **제너레이터 함수**(`function*`) 안에서 사용되는 키워드로, 함수의 실행을 일시 중지하거나 재개할 수 있게 한다. 얘 덕분에 제너레이터 함수는 값을 하나씩 생성하며 호출자에게 반환할 수 있다.

```js
function* counter(){
    console.log('round1');
    yield 1;
    console.log('round2');
    yield 2;
    console.log('round3');
}

const generatorObj = counter();

console.log(generatorObj.next());
console.log(generatorObj.next());
console.log(generatorObj.next());
console.log(generatorObj.next());
```

위 코드를 화살표함수로는 쓸 수 없는지 궁금했어.
그래서 찾아보니까 ==제너레이터는 화살표함수로 쓸 수가 없어==

그리고 `counter().next()`로 바로 쓸수도있지만, 그렇게하면 두번째 yield를 가져올 수가없어.
그러니까 한데 담아서 가져오는게 맞지.



# 충격적인 GENERATOR

```js
function* counter(){
    for(const v of [1,2,3]) {
        yield v;
    }
}
let generatorObj = counter();
console.log(Symbol.iterator in generatorObj);
for(const i of generatorObj){
    console.log('Obj:' + i);
}

// 놀랍게도 이것도 성립한다✅✅✅✅✅✅✅
function* counter2(){
    yield* [1,2,3];
}

let generatorObj2 = counter2();
for(const i of generatorObj2){
    console.log('Obj2:' + i);
}


generatorObj = counter();

console.log('next' in generatorObj);

console.log(generatorObj.next());
console.log(generatorObj.next());
console.log(generatorObj.next());
console.log(generatorObj.next());
```


## yield*? 이게 성립한다고?

```js
function* counter2(){
	yield* [1,2,3];
}
```

OK! 물론 성립하지. `yield*`은 현재 제네레이터의 실행을 다른 이터러블이나 제네레이터에게 위임한다는 뜻이야. `yield*` 의 뒤에 오는 이터러블 객체의 모든 값을 순회할 것이고. 각각의 값을 하나씩 현재 제네레이터의 yield지점으로 전달할 수 있다는 거거든. 

크게 두개의 의미를 갖는다고 할 수 있겠네. 

1. 위임: 내 뒤에 오는 어떤놈의 요소를 모두 순회하여 하나씩 뽑아오겠다 - 이터러블이 와야대
2. 반복: 내부적으로 이터러블 객체를 순회하며 그 값을 하나씩 반환한대.  for...of처럼 말이야. 



# 제네레이터 함수 정의

```js
function* genDecFunc(){
	yield 1;
}

let generatorObj = genDecFunc();
const genExpFunc = function* () {
	yield 1;
};
generatorObj = genExpFunc();

const obj = {
	* generatorObjMethod(){
		yield 1; 
	}
}

generatorObj = obj.generatorObjMethod();

class MyClass {
	* generatorClsMethod(){
		yield 1;
	}
}

const myClass = new MyClass();
generatorObj = myClass.generatorClsMethod();
```

()=>{} 이 형태는 적을 수 가 없다
그 내부에서 this를 쓸 수도 없고 별 기도호 못쓰니까.


# 이터러블을 제네레이터로 바꿔서 볼까?

```js
const infiniteFibonacci = ( function*(){
    let [pre, cur] = [0,1];

    while (true){
        [pre, cur] = [cur, pre+cur];
        yield cur;
    }
}() );

for(const num of infiniteFibonacci){
    if(num>10000) break;
    console.log(num);
}

console.log('***********');

const createInfiniteFibByGen = function* (max) {
    let [prev, curr] = [0,1];

    while (true) {
        [prev, curr] = [curr, prev+curr];
        if(curr>= max) return; // 오 그래?? 그럼 이제 끝내!!!
        yield curr; // 끝내지는말고 일단이거 리턴하고 정지 (다음에 부르면 다음꺼나감)
    }
};

for(const num of createInfiniteFibByGen(30000)){
    console.log(num);
}
```

