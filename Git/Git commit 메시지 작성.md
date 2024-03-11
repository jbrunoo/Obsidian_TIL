출처: [https://xtring-dev.tistory.com/entry/Git-규칙적인-Commit-메세지로-개발팀-협업하기-👾](https://xtring-dev.tistory.com/entry/Git-%EA%B7%9C%EC%B9%99%EC%A0%81%EC%9D%B8-Commit-%EB%A9%94%EC%84%B8%EC%A7%80%EB%A1%9C-%EA%B0%9C%EB%B0%9C%ED%8C%80-%ED%98%91%EC%97%85%ED%95%98%EA%B8%B0-%F0%9F%91%BE) 
[commit 메시지 영어 사전](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)


```
$ <type>(<scope>): <subject>    -- 헤더
  <BLANK LINE>                  -- 빈 줄
  <body>                        -- 본문
  <BLANK LINE>                  -- 빈 줄
  <footer>                      -- 바닥 글
```

type :
```
feat : 새로운 기능에 대한 커밋
fix : build 빌드 관련 파일 수정에 대한 커밋
build : 빌드 관련 파일 수정에 대한 커밋
chore : 그 외 자잘한 수정에 대한 커밋(기타 변경)
ci : CI 관련 설정 수정에 대한 커밋
docs : 문서 수정에 대한 커밋
style : 코드 스타일 혹은 포맷 등에 관한 커밋
refactor : 코드 리팩토링에 대한 커밋
test : 테스트 코드 수정에 대한 커밋
```


한 줄 안에 설명할 수 있도록 하나의 action마다 하나의 commit을 달기.
여러 디렉토리를 한꺼번에 수정하고 commit하지 말기.


[git commit message](https://github.com/joelparkerhenderson/git-commit-message)
