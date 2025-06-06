
# git init

버전관리를 시작하겠다고 선언

---
# git status

버전관리가 되고있는 내용들을 볼 수 있다
여기서 어떤파일들이 관리되고 있는지 확인가능

---
# git add

내가 추적하고싶은 파일을 git에게 알려준다.
프로젝트를 하다보면, 꼭 관리해야하는 파일만을 관리하는것이 좋다
그렇기때문에 관리하려는 파일을 정확히 체크해서 알려주는것이 좋다

## 왜  add를 꼭 해야할까?

여러 코드를 수정하다보면 커밋하는 시기를 놓칠수가 없다.
커밋하나는 하나의 작업을 가지고있는게 이상적이지만, 커밋시기를 놓치면 그러기가 어렵다
그럴때 우리는  add를 이용해서 딱 내가 원하는 것만 골라서 커밋할 수 있다는 장점이있다
git status에서 초록색으로 나오는 파일만 커밋이 될 것이라고 알려주는 것이다.
==선택적인 커밋==이 가능하다

## stage area

add 해놓아서 커밋 대기에 있는 파일들은 stage area에 들어가게된다. 이를 ==커밋대기상태==라고 할 수 있다. 
커밋 되는 파일이 저장되는 곳이 repository이다.(나중에 추가학습할 것이다)



---

버전: 의미있는 변화, 단위 작업이 완결된 상태

---


# User Setting

```
git config --global user.name yoondd 
got config --global user.email yoondd@kakao.com

```
위 내용은 딱 한번만 하면된다.
다른사람에게 누가 작업했는지를 알려주는 것.

---
# git commit

정보를 알려준다. 
현재 버전의 메시지를 가장 위쪽에다가 적어주면된다.

왜 바뀌었는지, 어떤것들이 변경되었는지의 커밋메시지를 입력해준다.
-> 이걸 쓰면 바로 버전을 만들게 되는 것이다


```
git commit -a
```

위 명령어를 치면 자동으로 add 기능까지 할 수 있다.


## commit이 가지고 있는 주요한 정보들

1. 이전 커밋이 누가인가. 부모를 가리키는 parent값.
2. 커밋이 일어난 시점에 우리의 작업 디렉토리에 있는 파일들의 이름과 파일들이 가지고 있는 내용이 무엇인가
즉, 버전이 만들어진 시점에 프로젝트 폴더의 상태를 확인할 수 있다 - 이를 ==스냅샷==이라고하고, 트리를 통해 스냅샷을 확인할 수 있다. 


---

# git log

어떤 변화가 일어났는지 그 로그를 확인할 수 있다.
역사를 확인하는 방법이다


---


# 버전을 만들면, 이제 무얼 할 수 있을까? 효용이 무얼까.

1. 차이점을 알 수 있다
2. 과거로 돌아갈 수가 있다.

# 1. 차이점을 확인할 수 있다

`git log`명령을 이용해서 커밋메시지를 포함한 역사들을 확인할 수 있다.
`git log -p` 명령어를 이용하면 각각의 커밋사이의 소스의 수정사항을 보여준다.

```
diff --git a/f1.txt b/f1.txt
index 1ef490d..56e7ceb 100644
--- a/f1.txt
+++ b/f1.txt
@@ -1 +1 @@
-source: 2
+f1.txt: 4
```

이런식으로 나온다. 자세히보니, ---a라고 적힌건 이전파일, +++b라고 적힌건 새로운 파일이라고 볼 수 있다
변화가 어떤게 일어났는지 정확하게 확인할 수 있게 되는 것이다

## git diff

각각의 커밋은 고유의 아이디가 있다. 복잡하게 생긴 엄청 긴 값. 그게바로 고유한 값이다
`git commit <고유한 아이디>`를 적어주면 해당내용이 보인다.

```
git diff 9fe6fc69ea2b55968e329aefe92d33fbd1e422ad..92ae35651e6ffe2621adc69c564c02420ca85ef0
```

위와같이 정확하게 어떤 커밋들 사이의 차이를 확인할건지를 정확히 입력할 수도 있다
간단히 문법을 정리하면 `git diff <commit id>..<commit id>`이다.

```
git diff
```

사실 그냥 이렇게만 입력했어도, 내가 커밋하기전에 내가 작업한 내용을 먼저 확인해낼 수 있다. 
이는 마지막으로 리뷰할 수 있는 기회가 된다. 여기에는 아직  add 하지 않은 내용이 나온다.



# 2. 과거로 돌아갈 수 있다

커밋을 취소하는 명령이라고 볼 수 있는데 주의를 많이 해야한다.
돌아가는 크게 두가지 방법이 있다

1. reset
2. revert

## git reset

```
git reset dadbd454013eed7f3587ba1b30bf25f0b11d9c8a --hard
```

해당내용의 커밋으로 돌아가게된다. 버전 두개를 버린 것 처럼 보이지만 사실은 남아있다.
복구할수는 있지만 어려운 내용이니까 나중으로 넘기자.
일단은 버전을 버리고 저 커밋id로 돌아갈 수 있다.

그런데 여기서 너무나 중요한 내용.
==절대로 공유한 파일을 reset하지말아라== 
원격저장소를 배우고 협업을 할 때, 공유를 할 수 있는데 그때는 절대로 reset을 해서는 안된다

저 위에서 나오는  hard는 굉장히 위험하지만, 단순하고 강력한 방법이기는 하다.
나중에 옵션을 좀 더 알아보자

## git revert

커밋을 취소하는건 맞지만, 커밋을 휙 날려보내는 것이 아니라, 해당 커밋을 취소하면서 새 커밋을 만드는 것이라고 볼 수 있다. 현재는 그냥 그런줄만 알자. 



