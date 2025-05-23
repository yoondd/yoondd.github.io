iptables는 보디가드야.
보디가드는 계속 인이어로 얘기를 들어야해
왜냐면 정책이 계속 바뀔수가 있잖아

쉽게말해, 프로그래밍적으로 보자면
런타임방식인거야. 컴파일방식이라기보다는.
(이해하기 쉽게말이야. 사실 정확히는 커널방식이고.)

### persistent - 영속성
원래 날아가는 정보인데 어딘가에 영구적으로 적어놓고 필요하면 불러다 쓰는거야
프론트 백 다 쓰는 용어다
자바에서 db날아가지않게 써놓는거랑 똑같아


## 1. OS단에서 정책을 넣어주는것 부터!

```sh
sudo iptables -A INPUT -p tcp --dport 20000 -j ACCEPT
sudo netfilter-persistent save
```

방화벽은 OS레벨에서 제어하는거니까 sudo를 붙여야해
대쉬(-)가 붙으면 다 옵션이야. 
키랑 밸류로 쪼개면된단 얘기야.
`-A INPUT`이랑, `-p tcp`이런게 다 키밸류로 세트로 가면된다는 말이야

#### -A INPUT
어디에 추가할거야? input? output? forward?

#### -p tcp
확인하라고 붙은거야. 통신유형이 tcp라는 거야.
ip를 확인하려면 프로토콜이 무조건 tcp여야하잖아.
UDP면 큰~일나. 확인을 못해

#### -dport 20000
도착하는 포트를 말하는거야.

#### -j ACCEPT
뭔 계획을 가지고있니 j야^^ 응. ACCEPT!
원래 INPUT에 디폴트가 ACCEPT거든?
근데 여기다가 ACCEPT를 넣어버리잖아?
==그러면 내가 지금 넣은 20000번으로 들어오는것만 허용할거니까 나머지 다 꺼져라.==
라고 세팅하는것과 똑같아.


여기까지 했으면, `show iptables -L`해서 확인해봐.

---


자, 여기까지하면 이제 20000번으로 들어오는것까지는 세팅이 된거야.
POSTGRES는 서버야. 서버 자체적으로도 보안이 있어.
(쉽게 말해, 대한민국 차원에서 테러범을 막아주기도하고, 우리집에도 안전장치를 거는것과 같아.)

그러니 내가 지금 설정한 것만으로 무조건 접근이 되는게 아니라
POSTGRES쪽에서도 해제 절차를 진행해야해.

여기까지가 OS단에서의 방화벽 해제였어.
이제 컨테이너단에서 방화벽을 풀어줘야하는 절차야.

*쉽게 말해 내 컴퓨터에 DB 도커를 올려뒀거든? 옆에 앉은 팀원이 내 도커 DB에 접근을 할려면 이러고 끝나면안된다는거야. 여기까지 한 것은 내 컴퓨터에 접근하는 방법이고, 내 DB에 접근하려면 그 이후 절차가 필요하다는 거지.*



## POSTGRES 설정바꾸기

컨테이너 안쪽으로 들어가서 그 내부의 OS에 접근해야한다는거야.

```sh
docker ps -a
```

내 컨테이너를 확인하고, 이제 내 컨테이너 내부로 들어가볼까?

```sh
docker exec -it test-db bash
```

이제 내부로 들어가겠네. `/#`가 깜빡깜빡 거리거든? 이제 내부로 들어왔다는 소리야.

```
root@djksdjfkdf:/# 
```

이렇게 나올거야. @ 뒤에는 그냥 아무거나 친거고^^;
샵이 나오잖아? 이거는 주인을 의미하는거야. 
달러나오면? 돈벌기위해 출근한 노예라고.

그러니까 이제 샵 나왔으니까 sudo같은거 안써도되는거야
여기서는 편집기가 없어서 편집기부터 좀 깔자고.

```
root@djksdjfkdf:/# apt install nano
```

nano를 깔아서 일단 편집할 수 있게 하자고.

```
root@djksdjfkdf:/# cat /var/postgresql/data/postgresql.conf | more
```

esc눌러서 그만읽을 수 있고, 너무 길어서 볼게 너무 많아 ㅠ

```
root@djksdjfkdf:/# cat /var/postgresql/data/postgresql.conf | grep listen_address
```

이렇게 하면 `listen_addresss = '*'`이라고 나오거든. 
이거는 방화벽은 아니야. 어떤거랑 통신할래? 에 대한 응답이야.
그냥 전체랑 통신할 수 있게 되어있다는거야. 자 여기까지 봤으면 됐어. 

이제 편집해보자.

```
root@djksdjfkdf:/# cd /var/lib/postgresql/data/
```

```
root@djksdjfkdf:/# nano ./pg_hba.conf
```

자, 여기까지 치고 nano로 연 파일을 내리다보면 익숙한 부분이 보인다.

방화벽 설정부분이다.

```
host all all 0.0.0.0/0 md5
```

md5는 암호화다.
여기서 잠깐 나오는 0.0.0.0/0부분있잖아. 이부분은 사이다표기법을 말하는거다. - [[Cidr]]
쪼갤게 없다. 이미 전체다. 전체 허용이다.

이걸 추가해주고 ctrl-o / ctrl-x눌러서 나온다.
이제 `cat`으로 보면 방화벽이 추가되어있을 것이다.

이제 `exit`눌러서 나가고, 컨테이너를 다시한번 재시작해준다 - 재부팅이 안되니까.

```sh
docker restart test-db
```


---


## 다 했다면 이제 접속해야겠지?

일단 나의 내부 아이피부터 확인해야겠어.

### 아이피부터 확인

```sh
ipconfig
```

나의 내부아이피를 확인하고. - 192-.168.0.193 확인완료.

여기서 broadcast가 나오거든?
방송국에 요청을 하는거야. 가능하냐고. 
방송국이 대역을 가지고있고, 해당 대역의 가장 마지막번호가 방송국꺼야.
방송국에게 허락받고 해당 내부 아이피를 부여받는거야.


### POSTGRES에서의 접속

우리 맨날 DB접속할 떄 (데이터그립에서) 127.0.0.1으로 접속했거든?
그거는 공구맘한테 요청을 안하는거였어.
127.0.0.1(로컬호스트)로 요청하면 공구맘한테 갔다가 "야 그거너잖아" "아 나네!" 이러고 다시돌아와
그래도 방화벽설정이 안되어있으면 접속안돼. 왜냐면 공구맘한테 갔다가못오는거야 바보처럼,

HOST: 내부아이피입력
이제는 로컬호스트로 접속하는거 아니고 내부아이피로 들어가면되는거야.


### 이제 yml파일의 내용을 바꾸면되겠다
거기서 내부 아이피로 변경하면 끝난다는거야.
팀원중에 한명만 도커를 켜고, 그 컴퓨터를 열어서 확인하면 된다는거야.

