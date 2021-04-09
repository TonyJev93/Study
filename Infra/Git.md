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


### Git Commit 규칙


#### Commit Message의 규칙
**7가지 규칙**

1. 제목과 본문을 빈 행으로 구분한다
2. 제목을 50글자 내로 제한
3. 제목 첫글자는 대문자로 작성
4. 제목 끝에 마침표 넣지 않기
5. 제목은 명령문으로 사용하며 과거형을 사용하지 않는다
6. 본문의 각 행은 72글자 내로 제한
7. 어떻게 보다는 무엇과 왜를 설명한다

```
type(타입) : title(제목)

body(본문, 생략 가능)

Resolves : #issue, ...(해결한 이슈 , 생략 가능)

See also : #issue, ...(참고 이슈, 생략 가능)
##################################################
    types = {
      feat : 새로운 기능에 대한 커밋
      fix : 버그 수정에 대한 커밋
      build : 빌드 관련 파일 수정에 대한 커밋
      chore : 그 외 자잘한 수정에 대한 커밋
      ci : CI관련 설정 수정에 대한 커밋
      docs : 문서 수정에 대한 커밋
      style : 코드 스타일 혹은 포맷 등에 관한 커밋
      refactor :  코드 리팩토링에 대한 커밋
      test : 테스트 코드 수정에 대한 커밋
    } 
```

- 참고 : [[Git] Commit message 작성 규칙](https://velog.io/@djh20/Git-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90)
