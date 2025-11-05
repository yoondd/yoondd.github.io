
## 맥북에서 git을 어디서든 실행할 수 있도록 PATH(환경변수) 설정하는 방법

**1. 현재 사용하는 셸 확인**

터미널을 열고 아래 명령어를 입력합니다.

bash

`echo $SHELL`

- `/bin/zsh`가 나오면 Zsh, `/bin/bash`가 나오면 Bash입니다[4](https://itvillage.tistory.com/entry/Mac-OS%EC%97%90%EC%84%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EB%93%B1%EB%A1%9D%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)[5](https://everyday-joyful.tistory.com/entry/%EB%A7%A5%EC%97%90%EC%84%9C-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0).
    

**2. 환경변수 파일 열기**

- Zsh 사용 시:
    
    bash
    
    `vi ~/.zshrc`
    
- Bash 사용 시:
    
    bash
    
    `vi ~/.bash_profile`
    

**3. git 실행 파일 경로 확인**

Homebrew로 git을 설치했다면 보통 `/opt/homebrew/bin`에 있습니다.  
직접 설치한 경우 `which git` 명령어로 경로를 확인할 수 있습니다.

bash

`which git`

예시 결과: `/opt/homebrew/bin/git`

**4. PATH에 git 경로 추가**

환경변수 파일(`.zshrc` 또는 `.bash_profile`)에 아래 내용을 추가합니다.

bash

`export PATH="/opt/homebrew/bin:$PATH"`

- 만약 다른 경로라면 해당 경로로 바꿔서 입력하세요[11](https://leeaain.tistory.com/entry/macM1-Homebrew%EB%A1%9C-git-%EC%84%A4%EC%B9%98-git-%EB%B2%84%EC%A0%84-%EB%B3%80%EA%B2%BD%ED%95%98%EA%B8%B0)[19](https://jyeonjyan.tistory.com/8).
    

**5. 변경 사항 적용**

파일을 저장하고 나와서, 아래 명령어로 변경 사항을 적용합니다.

bash

`source ~/.zshrc`

또는

bash

`source ~/.bash_profile`

**6. 정상 동작 확인**

터미널에서 아래 명령어로 git이 정상적으로 동작하는지 확인합니다.

bash

`git --version`

버전 정보가 출력되면 설정이 완료된 것입니다.

> 요약:
> 
> 1. 터미널에서 사용하는 셸 확인
>     
> 2. 해당 셸의 환경설정 파일(`.zshrc` 또는 `.bash_profile`)에 git 경로를 PATH에 추가
>     
> 3. `source` 명령어로 적용
>     
> 4. `git --version`으로 정상 동작 확인
>     

이렇게 하면 맥북의 어느 경로에서든 git 명령어를 사용할 수 있습니다[4](https://itvillage.tistory.com/entry/Mac-OS%EC%97%90%EC%84%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EB%93%B1%EB%A1%9D%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)[5](https://everyday-joyful.tistory.com/entry/%EB%A7%A5%EC%97%90%EC%84%9C-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)[9](https://dogfootja.tistory.com/87)[19](https://jyeonjyan.tistory.com/8).