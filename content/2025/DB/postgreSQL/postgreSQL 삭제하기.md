공식사이트를 이용해서 설치한  postgreSQL을 삭제하려고한다. 
스프링부터 강의 영상에서 보고 그대로 설치했고, 설치당시에 비밀번호를 입력하라고하기에 잘 따라했는데 (초기 비밀번호는 postgres 였다) 자꾸 pgAdmin실행 시 비밀번호가 틀렸다는 안내메시지가 확인되었기 때문이다.

몇번을 다시 깔고 설치하기를 반복했으나, 그 과정이 잦아서 남겨본다.
슬프게도 앞으로도 수없이 반복해야할 수도 있기에말이다.

참고로 버전은 17버전이다.

1. 서버 중지
	사실 켠적도 없어서 중지할것도 없지만, pgAdmin을 종료했다.

2. 터미널에서 삭제
```
sudo /Library/PostgreSQL/17/uninstall-postgresql.app/Contents/MacOS/installbuilder.sh
```
나는 17버전을 사용중이므로 이렇게 넣고 비밀번호를 입력해서 삭제를 요청했다

3. Entire application을 선택해서 삭제했다.
	entire가 전체니까 모든걸 지운다는 의미인것같아서 그냥 이걸 선택했다. 그러고나니 뭔가 진행되는 듯한 느낌의 bar가 나온다.

4. 메시지 하나가 띡. 나온다
	The data directory (/Library/PostgreSQL/17/data) and service user account (postgres) have not been removed.
	이 말인 즉슨, 데이터 디렉토리와 서비스 사용자 계정은 삭제되지않았으니 수동으로 삭제하라는 소리다

5. 데이터 디렉토리를 삭제하고 postgreSQL관련 파일을 추가 삭제한다
```
sudo rm -rf /Library/PostgreSQL/17/data

sudo rm -rf /Library/PostgreSQL
sudo rm /etc/postgres-reg.ini
sudo rm -rf ~/Library/Application\ Support/PostgreSQL/

```

6. 터미널을 통한 postgres계정 삭제
```
sudo dscl . -delete /Users/postgres
```

7. 시스템 재부팅
```
sudo reboot
```




## 정말 잘 삭제되었는지 확인하는 방법


### 데이터 디렉토리 확인

```
ls /Library/PostgreSQL/
ls /usr/local/var/postgres
```


### 프로세스 확인

```
pgrep postgres
```


### 계정 확인

```
dscl . -read /Users/postgres
```