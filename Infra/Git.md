# Git

## Basic

### Repository 등록

1. Git에서 Repository 신규 생성
2. 신규 Project git 초기화
    1. git init
    2. git add README.md
    3. git commit -m "init"
    4. git remote add origin {git-repository-url}
    5. git push -u origin master

### 신규 Branch 원격 등록

```
git push --set-upstream origin {branch_name}
```

### Push

- 현재와 다른 Branch에 push
```
git push <remote> <local_branch>:<remote_branch>
```
