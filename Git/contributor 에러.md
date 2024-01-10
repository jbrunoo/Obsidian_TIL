가장 처음 했던 프로젝트에 직접 organization을 만들었는데 contributor에 내가 없어서 찾아보았다.
내 github 닉네임은 jbrunoo인데 bruno로 커밋이 되어있고 회색으로 표시되어 있었다.

[공식문서](https://docs.github.com/ko/github-ae@latest/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-settings-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile)에 따르면 여러 경우가 있는데, 로컬 git config에 지정된 이메일과 github에 로그인한 이메일이 다를 경우 생기는 문제에 해당되었다.

bruno로 커밋된 페이지 url 주소에 .patch를 붙여주면 commit할 때 사용된 이메일 주소를 확인할 수 있다. 
github 로그인 이메일이 아닌 다른 이메일 계정이 적혀있었다..

중간에 여러 일들이 있어서 직접 설정했는지 기억은 안나지만 local config와 깃헙 이메일 모두 같은 이메일로 등록이 되어있었다. 즉, local config를 다른 이메일로 지정했던 커밋 기록이 바뀌어지지 않는다.

그래서 github settings -> emails 들어가보니 본인 이메일 여러 개를 추가할 수 있다.
그 당시 커밋했던 이메일을 추가해주었다. commit은 바로 인식되었고 contributors 1명 늘어나긴 했는데 아직 보이진 않는다. 