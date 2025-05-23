
# Node js가 무엇일까?
Node.js은 Chrome V8 Javascript엔진으로 빌드된 자바스크립트 런타임입니다.

## 런타임이란
특정 언어로 만든 프로그램들을 실행할 수 있는 환경

즉, Node js는 자바스크립트 프로그램을 컴퓨터에서 실행할 수 있게 해준다.
다시 말해서 ==자바스크립트 실행기==라고 할 수 있는 셈이다.

*그동안에는 브라우저가 자바스크립트 런타임을 내장하고 있으므로 자바스크립트 코드를 실행할 수 있었지만 일반 컴퓨터에서는 런타임이 없어 실행할 수가 없었다. 그래서 나온게 바로 Node js라는 소리다.*


---


# Node js의 주요한 특징


## 이벤트 기반
이벤트 기반이란 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식을 말한다.
클릭이나 네트워크 요청 등을 우리는 이벤트가 일어났다고 부르는데, 이러한 이벤트 기반 시스템에서는 특정 이벤트가 발생할때 대체 뭘 해야하는 것인지 ==미리 등록==을 해야한다. 
이것을 우리는 **이벤트 리스너** 또는, **콜백함수**를 등록한다고 한다. 

즉, 노드는 이벤트가 발생하면 이벤트 리스너에 등록해둔 콜백함수를 호출해서 진행한다.
발생한 이벤트가 없거나 발생했던 이벤트를 다 처리해버린다면 Node는 다음 이벤트가 발생할때까지 대기하고있다. 

Node js는 자바스크립트 코드의 맨 윗줄부터 한줄씩 실행하다가 ==함수 호출==을 만나면 호출한 함수를 ==호출 스택(Call Stack)==에 담는다.

### Call Stack
호출스택은 함수가 호출되었을때 담아놓는 스택구조의 상자다. 일단은 맨 처음에 anonymous라는 최초 실행시의 global context이다. 얘는 무조건 함수호출시 가장먼저 생성되는 환경이므로, 그냥 기본적으로 자바스크립트 코드 실행시에는 이 global context 안에서 돌아간다고 봐도 무방하다. 함수들이실행되는 동안 global context는 call stack안에 있다가 실행이 완료되면 지워진다. 

### Event Loop
이벤트 발생시에 호출할 콜백 함수들을 관리하고, 호출된 콜백 함수들의 실행 순서를 결정한다. 노드가 종료될 때까지 이벤트 처리를 위한 작업을 계속 반복하니까 루프라는 표현을 쓴다. 

### Background
setTimeout과 같은 타이머나 이벤트 리스너들이 대기하는 곳이다. 자바스크립트가 아닌 다른 언어로 작성된 프로그램이라고 봐도 무방하다. 여기선 여러작업이 동시에 실행가능하다.

### Task queue
이벤트 발생 후, 백그라운드에서는 태스크 큐로 타이머나 이벤트리스너의 콜백함수를 보낸다 정해진 순서대로 콜백들이 줄을 서있으므로 콜백 큐라고도 한다. 콜백들은 보통 완료된 순서대로 줄을 서있지만 특정한 경우 순서가 바뀌기도 한다. 



## Non-blocking I/O

우선은 블로킹과 논블로킹의 차이부터 알아야한다.
1. 블로킹 - 이전 작업이 끝나야만 다음 작업을 수행한다
2. 논블로킹 - 이전 작업이 완료되던가말던가 그다음작업이 실행된다

Node js에서는 파일의 입력 출력(I/O라고 부른다)을 할 때, 즉, 파일 시스템 접근이나 네트워크를 통한 요청 같은 작업을 할 때 ==논블로킹==방식을 택한다. 논블로킹 방식은 블로킹방식보다 더 짧은 시간에 처리할 수 있다는 장점이 있다. 다만!! 작업들이 모두 동시에 처리될 수 있는 작업이라는 전제가 있어야한다.

Node js에서는 I/O 작업을 위에서 배운 백그라운드에 넘겨서 동시에 처리한다. 동시에 처리될 수 있는 작업들을 최대한 묶어서 백그라운드로 넘겨야 시간을 절약할 수 있다. 



## 싱글 스레드

그 유명한 싱글스레드는 스레드가 하나 뿐이라는 의미다.
내가 만든 자바스크립트 코드가 동시에 실행될 수 없다는 것도 스레드가 하나뿐이기때문이다.

먼저, 잠시 프로세스와 스레드의 차이를 한번 알아보자
1. 프로세스 - 운영체제에서 할당하는 작업의 단위다. 노드나 브라우저같은 프로그램은 개별적인 별도의 프로세스들이다. 이런 프로세스간에는 메모리등의 자원을 공유하지않는다. 
2. 스레드 - 프로세스 내에서 실행되는 흐름의 단위이다. 프로세스는 스레드를 여러개 생성해서 여러작업을 동시에 처리할 수 있다. 같은 스레드끼리는 자기들을 묶어주는 부모 프로세스의 자원을 공유할 수 있다. 

노드는 원래 실행 후 프로세스 하나 만들고 여기에 스레드들을 생성하는데, 내부적으로 여러개를 생성하기는 하나 내가 직접 건드릴 수 있는 스레드는 오직 하나 뿐이다. 그래서 싱글스레드라고 할 수 있는 것이다. 

> 점원이 한 손님의 주문을 받고, 주방에 주문 내역을 넘긴 뒤에 다음 손님의 주문을 받는다. 요리가 끝나기까지 기다리는 대신 주문이 들어왔다는 것만 주방에 계속해서 알려주는 것이다. 주방에서 요리가 완료되면 완료된 순서대로 손님에게 서빙한다. 요리가 오래걸리는건 좀 늦게 나올 수도 있다. (블로킹인지 논블로킹인지에 따라 완료되는 순서가 다를 수 있다). 

바로 위 내용이 싱글 스레드, 즉 논블로킹 모델이다. 



---

# 노드를 서버로 쓰면 무엇이 좋은가?

- 서버에는 I/O 요청이 많이 발생하는데, 노드는 이를 논블로킹 처리하기때문에 스레드 하나가 많은 수의 I/O를 혼자서 감당한다. 
- 개수는 많지만 크기는 작은 데이터를 실시간으로 주고받을 때 최적화 되어있다
- 멀티스레드보다 프로그래밍이 상대적으로 쉽다
- 웹서버가 내장되어있어서 nginx같은걸 따로 설치할 필요가없다

