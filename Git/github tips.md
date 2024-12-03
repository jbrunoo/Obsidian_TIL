Issues 템플릿 만들기
- 해당 Repo - Settings - Features - Issues - Set up templates 여러 개 등록 가능
- 
PR 템플릿 만들기
- 위에서 만들어졌거나 또는 직접 .github 폴더를 최상위에 만들어주고
- PULL_REQUEST_TEMPLATE.md 파일을 만들어준다.

readme 프로젝트에 tree 구조도 그리기
```shell
brew install tree
cd 해당 repo
tree
copy tree
```

github pr local로 가져와서 테스트 [참고 블로그](https://dirtycoders.net/check-out-github-pull-request-locally/)
```
// 직접 명령어로 가져오기
git fetch origin pull/{num}/head:pr-{num}

// fetch 시, pr까지 자동 등록
// .git/config 파일 [remote = "origin"] 부분에 아랫줄 추가
fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```
