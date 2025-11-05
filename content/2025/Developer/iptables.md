리눅스의 방화벽 설정 iptables에 대해 알아보자

```sh
sudo iptables -L
```

이렇게 하면 방화벽 내역이 다 나온다.
현재 규칙을 확인할 수 있다.


# Chain
순서를 말하는거다.

### INPUT

```sh
Chain INPUT (policy ACCEPT)
target     prot opt source               destination   
```

INPUT은, 들여보내줄까? 라는 의미이다.  
정책이 안보이네. 
그러면 모두가 들어올 수 있다는 말이다. (전체허용)
지금 내용이 없거든.


### FORWARD

```sh
Chain FORWARD (policy DROP)
target     prot opt source               destination         
DOCKER-USER  all  --  anywhere             anywhere            
DOCKER-FORWARD  all  --  anywhere             anywhere   
```



policy DROP으로 되어있으면 디폴트가 막는거야.
아무도 못들어온단 말이야.

로컬호스트로 컨테이너에 접속하면 기본적으로 DROP이라서 못들어간단말이야
근데 내가 설정하지도 않았는데 어떻게 들어가냐고? 
도커가 기본으로 넣어줬거든. `DOCKER-USER  all  --  anywhere anywhere `
이렇게 허용을 기본적으로 해줬다는 말이야. 

**기본적으로 DROP정책이기때문에 이 목록에 들어가는건 전부 허용에 대한 이야기다**

그러니까 여기 설정들을 이용해서 컨테이너에 접근할 수 있었다는 말이야.



### OUTPUT

```sh
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

서버에서 밖으로 나가는 것을 말한다

왜 필요하냐고?
트로이목마(은밀한 침투) 바이러스 생각해봐.
숨어가지고 계속 감시하면서 훔쳐보고 그러거든?
정보를 밖으로 빼낼 수 있단말이야.
내 화면을 보고 타이핑하는걸 훔쳐보면서 외부로 빼낸단말이야
트래픽 감시하다가 특정아이피로 이상하게 내 정보가 새나간다?? 그럼 또 막아야할거아니야
그러니까 밖으로 나가는 것도 정책이 필요해.

기본정책은 ACCEPT 허용이거든?
그러니까 이 목록에 나오는 것은 밖으로 나가는것에 대해 막는 설정을 말하는 거지.



```sh
Chain DOCKER-CT (1 references)
target     prot opt source               destination         
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED

Chain DOCKER-FORWARD (1 references)
target     prot opt source               destination         
DOCKER-CT  all  --  anywhere             anywhere            
DOCKER-ISOLATION-STAGE-1  all  --  anywhere             anywhere            
DOCKER-BRIDGE  all  --  anywhere             anywhere            
ACCEPT     all  --  anywhere             anywhere            

Chain DOCKER-ISOLATION-STAGE-1 (1 references)
target     prot opt source               destination         
DOCKER-ISOLATION-STAGE-2  all  --  anywhere             anywhere            

Chain DOCKER-ISOLATION-STAGE-2 (1 references)
target     prot opt source               destination         
DROP       all  --  anywhere             anywhere            
```

여기에 보면, references라고 나오는것도 보이는데 
그 이름 갖다가 나중에 쓰겠다는거야. 
`DOCKER-ISOLATION-STAGE-1  all  --  anywhere   anywhere`
이런 식으로 말이야.



---


자, 우리는 학원에서 방화벽풀고 DB설정을 할거야.
그러면 모든 이가 내 DB를 이용할 수 있을까아~?
그리고 그 설정을 어떻게 하는건데~?
이 모든 내용은 

공유기에 대해 이해해야하고, 
또, 그 방법에 대해 알아야해. 
그 방법이 바로 ==포트포워딩==이라는건데.

이 내용들이 중요해보여서 나는 따로 정리를 해봤다.

[[Port Forwarding]]

결국 우리가 공구맘한테 등록을 해줘야한다는거다.
그 작업은 우리가 학원 공유기에 포트포워딩 작업을 해야한다는 건데.

우리가 학원 공유기를 만지면안되니까, 우리가 아무리 서버를 켜봤자 밖에서 들어올 수가 없다는말이다.

즉, 우리가 서버 설정을 하는건 학원 내에서만 사용할 수 있는 설정이다.