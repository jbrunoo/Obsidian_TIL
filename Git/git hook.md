첫번째 사용이유는 팀프로젝트에서 Jira issue 번호를  prepare-commit-msg에 포함시키기 위함이다.

프로젝트의 .git 폴더 하위에 hooks 폴더에 여러 sample들이 있다.
.sample 확장자를 떼고 사용하거나 새로 지정해주면 된다.

git hook 파일들이 공유가 안된다는 말이 있던데
팀원이 보내준 파일을 그대로 넣었더니 문서로 저장되었다.

```
chmod +x <파일 경로>
chmod +x .git/hooks/commit-msg
```
터미널에 다음 명령어를 적으면 Unix 실행파일로 전환된다.