branch를 만들었을 때 현재 어떤 상황인지를 알아야하니까.

# 각 branch사이의 차이를 알고싶을 때.

```bash
git log --branches --decorate
```

내가 가진 모든 브랜치의 log를 알 수 있다
별로 좋아보이지를 않지?

이때 바로 놀라운 상황을 볼 수 있어 

```bash
git log --branches --decorate --graph
```

이렇게 각자 도생의 길을 걸어갈 때 진짜 좋은 점들이 드러난다. 놀랍다

```bash
git log --branches --decorate --graph --oneline
```

한 줄의 라인으로 더 쉽게 볼 수 있게 해준다.


## 두 개 사이의 차이만 정확하게 보고 싶을 때,

```bash
git log main..exp
```

위와같이 입력하면  ==main에는 없고 exp에는 있는 것==들을 볼 수 있다.

만약에 소스코드 까지 보고싶다면

```bash
git log -p main..exp
```

-p옵션을 이용하면 소스코드까지 정확하게 보여준다.


```bash
git diff main..exp
```

각각의 브랜치의 현재의 상태를 비교할 수 있다. 


