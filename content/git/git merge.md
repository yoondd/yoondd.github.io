merge를 하는 과정에서 파일이 충돌났을때 내가 어떻게 해야할까?
깃이 얼마나 많은부분을 자동화해주는가.
기특하다.
충돌했어도 너무 당황하지말아라.

- 충돌이 생기면 merge커밋을 만들어낸다

# git merge happy

happy에 있는 수정본과 나의 브랜치를 합친다.
해피한상황에서는 각자의 역할이 분리된경우 문제될것이 없다.
해피한상황? - 아예 다른 파일을 건드리거나, 서로 다른 위치이거나.

# CONFLICT

같은 파일 같은 위치를 건드린 경우에 가장 슬픈 상황이 만들어진다

```
Auto-merging common.txt
CONFLICT (content): Merge conflict in common.txt
Automatic merge failed; fix conflicts and then commit the result.
```

말도안돼. 내가 다른 branch에서 같은 곳을 건드리다니!

그래, 이런상황에서는 파일로 진입했을때 다음과 같은 내용을 볼 수 있다.

```
function master(){}
<<<<<<< HEAD
function a(master){}
=======
function a(happy){}
>>>>>>> happy
function happy(){}
```

자, 여기서 구분자는 ======= 이다. 
해당 내용을 구분으로 위와 아래가 있다.
여기서 head는 현재 있는 브런치를 말하는거고, happy는 합치고자했던 happy브런치를 말하는거다.
그래 이경우에는 내가 직접 수정을 해야지.



## 결론

- git merge를 했을 때, 파일이 아예 다르면 문제없이 합쳐진다
- git merge를 했을 때, 같은 파일이라도 수정 위치가 다르면 알아서 auto merge된다
- 문제는 git merge를 했을 때, 같은 위치를 수정하는 그 순간이다. 이 때는 git이 우리에게 위임을 한다. 위에 나와있는 파일처럼 나오니까 이 상황에서는 ==== 의 위쪽 아래쪽을 알아서 내가 *직접* 합치고나서 add및 commit하면 끝난다.