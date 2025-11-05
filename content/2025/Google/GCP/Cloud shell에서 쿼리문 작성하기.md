# cloud run에서 간단히 sql문을 사용할 수 있는 방법?

```bash
gcloud config set project <프로젝트 아이디>
gcloud config set kpop-list-455808
```

```bash
gcloud sql connect root --user=root
```

이렇게 쓰니까 gcp쉘에서 sql을 사용할 수 있네 너무좋다! 굳이 디비버같은게 필요가없네.