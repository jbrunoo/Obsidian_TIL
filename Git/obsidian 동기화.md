다른 노트북으로 obsidian 동기화 옮길 시.
옵시디언 폴더를 onedrive를 통해 옮겨서 그대로 가져왔으나
obsidian 실행 시 obsidian_git plugin이 함께 가져왔는데 fatal : bad object HEAD 오류


1. .git이 있는 obsidian 폴더에서 git bash 실행.
	git status 쳐보면 fatal: bad object HEAD 똑같은 에러 메시지.

2. cp -R .git/objects.backup .git/objects 
	objects 파일에 백업 파일이 있어야만 가능.

3. git add . 을 해보면 모든 파일에 warning 뜸. LF will be replaced by CRLF the next time Git touches it.
- git commit -m "HEAD 문제 해결"
	fatal: could not parse HEAD
- git push origin main
	Everything up-to-date

4. git fetch origin 결국 원격저장소에서 최신 변경 사항을 가져옴으로써 해결된 듯하다.
	remote: Enumerating objects: 332, done.
	remote: Counting objects: 100% (332/332), done.
	remote: Compressing objects: 100% (258/258), done.
	remote: Total 332 (delta 150), reused 246 (delta 66), pack-reused 0
	Receiving objects: 100% (332/332), 7.68 MiB | 6.24 MiB/s, done.
	Resolving deltas: 100% (150/150), done.

5. git branch -f main origin/main
	git checkout main 
	이미 main 브랜치라고 뜬다.



추가)
맥북 구매 후 똑같이 구글 드라이브를 통해서 옮겼음
OS 변경, 사용자명도 바꿔서 약간 다른 fatal로 시작하는 오류가 떴음
git fetch obsidian 하니 user name과 password가 필요한데
입력하면 이제는 password 대신  토큰 인증을 하라고 뜸

github - settings - developer mode 에서 토큰 생성하면 됨

`git remote set-url origin https://jbrunoo:<YOUR_TOKEN>@github.com/jbrunoo/Obsidian_TIL.git/`
이후 token을 저기 넣어주면 해결

