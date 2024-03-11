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




advanced
git cherry-pick <\commit1> <\commit2> ...
	현재 위치 아래에 있는 일련의 커밋들에 대한 복사본을 만듦
interactive rebase (rebase + -i 옵션)
	위의 체리 픽은 유용하지만 원하는 커밋(각각의 해쉬 값)을 모를 때. 리베이스할 일련의 커밋을 검토하기 가장 좋은 방법

ex1) 작업 브랜치에서 debug 등의 커밋을 수행 후 불필요한 커밋은 제외하고 합치고 싶을 때 cherry-pick 으로 특정 커밋만 고르거나 rebase -i main(HEAD~4) 처럼 gui에서 순서를 옮기거나 omit/pick 해서 합칠 수 있음.
cherry-pick은 수행되면 자동으로 main이 이동함. rebase는 수행 후 main 브랜치를 다시 rebase bugFix main 해주어야 함

ex2) 커밋1 커밋2 진행하다가 커밋1의 내용을 수정하고 싶을 때. rebase -i 를 통해 커밋 1의 순서를 앞당기고 commit --amend 후 다시 순서를 커밋1 커밋2로 바꿔줌. 커밋1' 커밋2' 커밋1'' 커밋2''이 증가하고 순서를 바꾸다가 리베이스 중 충돌 위험 존재. git cherry-pick 으로 떼어내서 붙여도 됨.

git tag 
git describe <\ref>

