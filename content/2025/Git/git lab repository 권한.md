git lab에 내 작업물을 업로드하고싶어서 다음의 단계를 거쳐 진행했다

1. git init
2. git add 
3. git commit
4. git remote
5. git push

그런데 push하는 단계에서 오류가 발생했다

```sh
Enumerating objects: 22, done.
Counting objects: 100% (22/22), done.
Delta compression using up to 8 threads
Compressing objects: 100% (22/22), done.
Writing objects: 100% (22/22), 9.68 KiB | 9.68 MiB/s, done.
Total 22 (delta 6), reused 0 (delta 0), pack-reused 0
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
To https://gitlab.com/study4233817/til.git
 ! [remote rejected] main -> main (pre-receive hook declined)
error: failed to push some refs to 'https://gitlab.com/study4233817/til.git'

```

권한오류다

***Setting - Repository - Protected branches***

여기 들어가서 보호를 끄면된다.
