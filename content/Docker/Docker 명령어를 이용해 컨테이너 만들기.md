# 이미지 가져오기
```bash
docker pull postgres:16
```

안정성을 위해 최신(latest)보다 한단계 낮은버전으로 가져오라.


# 이미지 확인하기
```bash
docker images
```


# 현재 있는 프로세스(컨테이너) 확인
```bash
docker ps -a
```

# 실행한다
```bash
-x
```


# 컨테이너 실행
```bash
docker run --name happy
```
--뒤에는 여러개의 문자가 오고, -뒤에는 문자 하나가 온다
--name은 어떤 이름으로 컨테이너를 실행할 것인가 이야기하면된다.
happy라는 이름으로 컨테이너를 만들어 실행하겠다는 뜻이다.

## 명령어의 형태
```bash
docker run <key> <value> <key> <value>....
```


# 환경변수 설정하기
```bash
docker run --name happy -e POSTGRES_USER=test_user
```

## e명령어 뭐지?
```bash
-e 하면 환경변수~
```

## 여러개 써도되나?
```bash
docker run --name happy -e POSTGRES_USER=test_user -e POSTGRES_PASSWORD=haha1234
```
물론! 여러개의 환경변수를 입력해도되지!



# 포트설정하기
```bash
docker run --name happy -e POSTGRES_USER=test_user -e POSTGRES_PASSWORD=haha1234 -p 10000:5432
```

## p명령어 뭐지?
```bash
-p 하면 포트번호를 말한다
-p <host port>:<container port>
-p 10000:5432 
```



# 백그라운드에서 작업하라고하기
```bash
-d 하면 백그라운드에서 작업하도록 명령

docker run --name happy -e POSTGRES_USER=test_user -e POSTGRES_PASSWORD=haha1234 -p 10000:5432 -d
```


# 어떤 이미지를 이용해서 컨테이너를 돌릴지 적어주기
```bash
docker run --name happy -e POSTGRES_USER=test_user -e POSTGRES_PASSWORD=haha1234 -p 10000:5432 -d postgres:16
```

자! 여기까지 하니까 알수없는 글자가 쭉~~ 나오네. 잘 실행되었나봐.


```bash
docker ps -a
```

위 명령어를 이용해서 잘 살펴보기.


# db를 다한 것 같은데 이제 어떻게 접속하지?

위와같은 방식에 따라 db를 도커로 만들어 올렸다면,
`localhost:10000` 이것이 db접속주소이겠네.
test_user / haha1234로 접속하면 되지.


# 두번째 프로젝트를 시작해서 db를 또 만들어야한다면?

```bash
docker run --name happy2 -e POSTGRES_USER=test_user -e POSTGRES_PASSWORD=haha1234 -p 10001:5432 -d postgres:16
```

컨테이너 이름이랑, host port를 다른걸로 세팅해주면되겠네.
이름이나 pw는 변경안해도돼. 어차피 ==격리==되어있잖아.



# 컨테이너 삭제
```bash
docker rm
```


# 컨테이너  중지/재실행
```bash
docker stop 424 #424...라고 아이디를 가진 컨테이너를 중지한다
docker start 424 #424...라는 아이디를 가진 컨테이너를 실행한다.
docker stop 424 0c8 #424... 0c8... 아이디를 가진 컨테이노 모두 중지
```

# 이미지 삭제
```bash
docker rmi 300
```
300뭐냐고?  `docker images`로 확인한 이미지의 id 야.  
그런데 이미지 삭제가 안될 수 도 있어. - 컨테이너를 중지하고 삭제하면돼  




# 컨테이너 3개를 실행하다가 컴퓨터를 껐다키면 컨테이너는 어떻게 될까?

중지돼. 왜냐면 한 10000개를 누가 악의로 실행시킨 다음에 이상해져서 내가 컴퓨터를 껐다킨거야. 근데 그대로 살아있어? 그러면 어떻게 되겠어. 또죽어!!! 그러니까 안전하게 컨테이너는 중지가 되었다가 다시 살아난다고 생각하면돼. 