
계속 mysql Workbench가 강제종료되는 문제가 발생했다.

```
brew install mysql
```

꽤 긴 시간에 걸쳐 mysql을 설치 한 후에 

```
brew services start mysql
```

위 명령어를 이용해 mysql을 실행시켰다

```
mysql -u root
```

루트 계정으로 접속하고나니 Workbench에서 접속이 가능해졌다.

---

참고로, mysql을 끄는 것은

```
brew service stop mysql
```