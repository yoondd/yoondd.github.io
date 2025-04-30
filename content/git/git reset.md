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
