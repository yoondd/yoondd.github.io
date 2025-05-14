
작업을 분기해서 작업했는데, 그래서 각자의 히스토리가 생겼는데 어느시점에서 병합을 해야하는 상황이 생겼을때, 어떻게 병합( merge)하는가다. 

# exp를 main으로 가져오려면.

우선  main 으로 체크아웃을 하고.

```bash
git merge exp 
```

이렇게 하면 exp의 내용이 main으로 가져와진다.

반대방향으로 하면 두 브랜치가 완전히 똑같애지겠지.

# branch 지우기

```bash
git branch -d exp
```





# Fast-forward  - 커밋을 생성하지않는다

빨리감기? 브랜치 분리후 메인에서는 작업안했는데 분리된거에서만 작업을 한거야.

![](https://i.imgur.com/5umhw5g.png)

> master에서 작업중이었는데, iss53을 만들어서 작업하던 중 급하게 처리해야할 일이 있어. 그래서 master로 간다음에 hotfix를 땡겨써서 작업을 했어. master에서는 아무 작업이 없었거든? 그래서 hotfix 작업 마치고 master로 간다면 작업한  hotfix 를 땡겨오는거지. 이게 바로 ==빨리감기==야. 

![](https://i.imgur.com/KT4xZYO.png)

바로 이렇게 되는거지. 따라서 별도의 커밋을 만들어 내지않아. 그냥 마스터가 가리키는 커밋이 누구인지를 바꾸기만 하면 되는거야. 이러고나서는 hotfix를 지워버리면 완벽하지. 


---


## Fast-forward가 아닌경우  - 커밋을 만든다


![](https://i.imgur.com/klWnZbe.png)

> 자 이제 급한 불 껐고, iss53을 작업을 했어. 자, 이제 iss53을 적용을 시킬건데 말이야. master로 간다음에 iss53을 불러들일거거든? 근데 뭐라고 나오냐면 `merge made by the 'recursive' strategy`라는 거야. 이게 무슨일이야?! 

![](https://i.imgur.com/ddqnkfT.png)

> iss53가 독립한 이후에 master이 변해버렸지뭐야. 이런 경우에는 fast forward가 안되는데? 

내가 돌아갈 자리가 지금 변해버렸어. 이 경우에는 어떻게 동작해야할까? 내부적으로 git은 이렇게 진행한다.

1. master 와 iss53의 공통의 조상을 찾는다
2. 3-bay merge라는 내부적인 방법을 이용해서 C4 와 C5를 합친다
3. 두개를 합친 별도의 커밋을 만들다 (C6) - 자동으로만드는거다. (c5,c4에서 비롯된 버전이라는것이  c6 에 담겨있게 되는 것이다)

![](https://i.imgur.com/BwDPi4l.png)


