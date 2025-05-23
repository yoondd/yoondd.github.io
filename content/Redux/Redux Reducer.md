```js
function reducer(state, action){  
    if( state === undefined ){  
        return { color: "yellow" };  
    }  
}  
const store = Redux.createStore(reducer);  
  
console.log( store.getState() );
```

Redux.createStore()로 은행을 만들건데,
은행을 만들때에는 무조건 reducer가 필요하다

reducer는 state와 action을 가지고 있고, 
우리는 최초 초기값을 지정했다.

이 상태에서 console에 찍히는것은 우리가 만들어준 초기값이다.

```js
function colorChange(){  
    const state = store.getState();  
    document.querySelector("#component_red").style.backgroundColor = state.color;  
    document.querySelector("#component_green").style.backgroundColor = state.color;  
    document.querySelector("#component_blue").style.backgroundColor = state.color;  
    document.querySelector("#component_orange").style.backgroundColor = state.color;  
}  
  
colorChange();
```

이제 이렇게 사용하면되는거지. 필요할때 getState로 불러서 사용하는거야!!


```js
function reducer(state, action){  
    console.log(state, action);  
    if( state === undefined ){  
        return { color: "purple" };  
    }  
    let newState;  
    if( action.type === "change_color"){  
        newState = Object.assign({}, state, {color: "red"});  
    }  
    return newState;  
}  
const store = Redux.createStore(reducer);  
  
function colorChange(){  
    const state = store.getState();  
    store.dispatch({ type: "change_color", color: "red" });  
}
```

복사해야지만 리덕스가 주는 혜택을 모두 사용할 수 있다.



## 공부한 내용

```html
<!doctype html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.1/redux.js" integrity="sha512-aU+9st6E3LYPknXJiOkhUXxYz/QbB1IDf1YUYzCCbgiwOCu2g/1pH+68ROdxC3clouCOVfO6u2g7qoB43rfmQg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <title>study redux</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        main {
            display: flex;
            justify-content: center;
            gap: 20px;
            text-align: center;
        }
        h1 {
            margin: 50px 0 20px;
            text-align: center;
        }
        .box {
            display: flex;
            flex-direction: column;
            gap: 10px;
            width: 150px;
            border: 3px solid black;
            padding: 20px;
        }
    </style>
</head>
<body>
    <h1>Redux Study</h1>
    <main>
        <div id="component_red" class="box">

        </div>
        <div id="component_green" class="box">
        </div>
        <div id="component_blue" class="box">
        </div>
        <div id="component_orange" class="box">
        </div>
    </main>


    <script>
        function reducer(state, action){
            console.log(state, action);
            if( state === undefined ){
                return { color: "purple" };
            }
            let newState;
            if( action.type === "change_color"){
                newState = Object.assign({}, state, {color: action.color});
            }
            return newState;
        }
        const store = Redux.createStore(reducer);

        function red(){
            const state = store.getState();
            document.querySelector("#component_red").innerHTML = `
                <div style="background-color: ${state.color}">
                <h2>Red</h2>
                <button id="red" onclick=" store.dispatch({type:'change_color', color: 'red'}); ">Change</button>
                </div>
            `;
        }
        store.subscribe(red);
        red();

        function blue(){
            const state = store.getState();
            document.querySelector("#component_blue").innerHTML = `
                <div style="background-color: ${state.color}">
                <h2>Blue</h2>
                <button id="blue" onclick=" store.dispatch({type:'change_color', color: 'blue'}); ">Change</button>
                </div>
            `;
        }
        store.subscribe(blue);
        blue();


        function green(){
            const state = store.getState();
            document.querySelector("#component_green").innerHTML = `
                <div style="background-color: ${state.color}">
                <h2>green</h2>
                <button id="green" onclick=" store.dispatch({type:'change_color', color: 'green'}); ">Change</button>
                </div>
            `;
        }
        store.subscribe(green);
        green();
    </script>
</body>
</html>
```


---

```js
const store = Redux.createStore(  
    reducer,  
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()  
);
```

이렇게 추가해두면, redux web dev 툴에서 시간여행이 가능하다

