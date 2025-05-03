<<<<<<< HEAD
![](https://i.imgur.com/6nvg4wh.png)

working directory -> 내 작업공간
index, staging area -> add 된 상태
repository ->커밋한 상태(버전들이 저장된 곳)

그냥 속편하려면 `git reset --hard`하면된다
=======
```bash
git log
git log --branches --graph --decorate
```

위 내용들을 이용해서 로그를 확인한 후에 거기나오는 commit아이디를 확인해라.
그다음에 그쪽으로 돌아가게 만들어야한다.

```sh
git reset <commit id>
```

이렇게 하면 그쪽으로 돌아갈 수 있게된다.


## 잠깐, 그러면 방금 내가 한거 삭제한거야?

reset은 취소못하나?
그럴리가. git은 웬만하면 삭제하지않아.
ORIG_HEAD가 가지고 있어.

```sh
git log --herd ORIG_HEAD
```

이렇게 하면 리셋을 취소할 수 있게되는거야.

이것보다 더 좋은 방법이 있는데, 

```sh
git reflog
```

위 내용을 치고나서 확인해보면 내가 어떤 활동을 했는지 알수있어. 



#### checkout을 이용해서 돌리기

```sh
git checkout <commit id>
```

특정한 브랜치에 속해있는것이아니라 특정 커밋레 detached되어있다 
뭐 이런경우도있는데, 굳이 알필요는없다. 
>>>>>>> origin/main
