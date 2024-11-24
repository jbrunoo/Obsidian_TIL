[learn git branching site](https://learngitbranching.js.org/?locale=ko)

git commit
git branch
git checkout (-b)
git rebase 
git checkout (commit / hash) - HEAD
git checkout HEAD^ / HEAD~1(num)
git branch -f main HEAD~3
git reset HEAD~1 : local용
git revert : remote용


- - -
advanced
git cherry-pick <\commit1> <\commit2> ...
	현재 위치 아래에 있는 일련의 커밋들에 대한 복사본을 만듦
interactive rebase (rebase + -i 옵션)
	위의 체리 픽은 유용하지만 원하는 커밋(각각의 해쉬 값)을 모를 때. 리베이스할 일련의 커밋을 검토하기 가장 좋은 방법
	git rebase -i HEAD~3(최초의 커밋 포함 시 git rebase -i --root), 이런 식으로 interactive rebase를 세션을 열고 pick -> squash 또는 s로 변경 해주어서 가장 가까운 commit으로 묶어 정리할 수 있음. 
	[안스 IDE에서 사용하기](https://www.youtube.com/watch?v=sAa4QxODhc4)
```git
pick aaaaaa hello
pick bbbbbb hello2
pick cccccc hello3

----
pick aaaaaa hello
s bbbbbb hello2
s cccccc hello3
```



ex1) 작업 브랜치에서 debug 등의 커밋을 수행 후 불필요한 커밋은 제외하고 합치고 싶을 때 cherry-pick 으로 특정 커밋만 고르거나 rebase -i main(HEAD~4) 처럼 gui에서 순서를 옮기거나 omit/pick 해서 합칠 수 있음.
cherry-pick은 수행되면 자동으로 main이 이동함. rebase는 수행 후 main 브랜치를 다시 rebase bugFix main 해주어야 함

ex2) 커밋1 커밋2 진행하다가 커밋1의 내용을 수정하고 싶을 때. rebase -i 를 통해 커밋 1의 순서를 앞당기고 commit --amend 후 다시 순서를 커밋1 커밋2로 바꿔줌. 커밋1' 커밋2' 커밋1'' 커밋2''이 증가하고 순서를 바꾸다가 리베이스 중 충돌 위험 존재. git cherry-pick 으로 떼어내서 붙여도 됨.

git tag 
git describe <\ref>


```
~/kotlin-lotto-7 jbrunoo +2 !2 ❯ git rebase -i HEAD~3                                                                                                                    05:02:24
error: rebase 할 수 없습니다: 인덱스에 커밋하지 않은 변경 사항이 있습니다.
error: 커밋하거나 스태시에 넣으십시오.

~/kotlin-lotto-7 jbrunoo +1 ❯ git stash                                                                                                                                  16:54:12
Saved working directory and index state WIP on jbrunoo: c0ec8bc refactor: 상수 값 추가 분리

~/kotlin-lotto-7 jbrunoo *1 ❯ git rebase -i HEAD~3                                                                                                                       16:55:19
[HEAD 분리됨 2d8355a] refactor: 상수 값 분리
 Date: Mon Nov 4 16:45:37 2024 +0900
 7 files changed, 48 insertions(+), 19 deletions(-)
 create mode 100644 src/main/kotlin/lotto/constants/Constants.kt
 create mode 100644 src/main/kotlin/lotto/constants/Exceptions.kt
Successfully rebased and updated refs/heads/jbrunoo.

~/kotlin-lotto-7 jbrunoo *1 ❯ git stash pop                                                                                                                      14m 59s 17:10:50
현재 브랜치 jbrunoo
커밋할 변경 사항:
  (use "git restore --staged <file>..." to unstage)
        새 파일:       src/main/kotlin/lotto/service/LottoService.kt

Dropped refs/stash@{0} (01c4255e69f9ad063b6ad0213a6dbb1165ab2fe3)
```
상수 값 분리를 꼼꼼히 안해서 3번 연속 커밋을 해버렸다.
rebase 실행 전, 변경된 파일이 없어야 한다.
그래서 커밋하거나 스태시에 넣으라는 에러가 떴다.
git stash -> 작업 -> git stash pop 을 통해 작업 내용을 임시 저장해둘 수 있다.

- - -
[서브 모듈과 서브 트리](https://jammdev.tistory.com/111)



- - -
```git
git restore . # 수정된 파일 초기화 
git clean -fd # 삭제된 파일 복구 및 추적되지 않은 파일 제거
```

git restore가 동작하지 않는 경우
```git
# 작업 영역에 스테이징되어 있음. 삭제된 파일을 git 스테이징 영역에서 제거.
git restore --staged <파일경로>
git restore <파일경로>

git status
```
