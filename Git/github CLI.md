pr 로컬 테스트를 위한 방법을 찾다가 발견하였다.

[blog1](https://blog.outsider.ne.kr/1204) [blog2](https://blog.outsider.ne.kr/1498)

pr 로컬에서 테스트하기.

1. .gitconfig를 수정하여 pr을 가져오는 별칭을 만들어준다.
2. [github CLI](https://cli.github.com/)를 사용한다. [quick-start](https://docs.github.com/en/github-cli/github-cli/quickstart)

- - -

설치 후 pr 관련 명령어 (타 명령어는 문서 참고)
```
brew install gh

gh auth login

gh pr list (--web)

gh pr checkout <pr-num>
```


ps. fzf 유틸리티 이용 시, 파일탐색 수월