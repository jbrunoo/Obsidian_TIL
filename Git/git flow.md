주 브랜치 : main(master), develop
feature : 단위 기능 개발, 기능 개발 완료 시 develop 브랜치 merge
release : 배포를 위해 main(master) 브랜치로 보내기 전 먼저 QA(품질검사) 하기 위한 브랜치
hotfix : main(master) 브랜치로 배포를 했는데 버그가 생겼을 때 긴급 수정하는 브랜치

issue에 등록된 description을 보고 위 naming 참고해서 새 브랜치 생성 후 PR 보내고 merge 작업
[issue & PR template](https://github.blog/2016-02-17-issue-and-pull-request-templates/) [참고 설정 블로그](https://amaran-th.github.io/Github/[Github]%20Issue%20&%20PR%20Template%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/)
[medium - 커밋 메시지에 issue 번호 자동 추가](https://medium.com/prnd/github-%EC%BB%A4%EB%B0%8B-%EB%A9%94%EC%84%B8%EC%A7%80%EC%97%90-jira-%EC%9D%B4%EC%8A%88%EB%B2%88%ED%98%B8-%EC%9E%90%EB%8F%99%EC%9C%BC%EB%A1%9C-%EB%84%A3%EC%96%B4%EC%A3%BC%EA%B8%B0-779048784037)
[10분 테코톡](https://www.youtube.com/watch?v=wtsr5keXUyE)
[merge, squash, rebase 블로그](https://velog.io/@emrhssla/%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-merge-no-ff-squash-rebase-%EA%B7%B8%EB%A6%AC%EA%B3%A0-pull-requestPR)

- - -
일반 merge는 merge --ff와 같음. ff는 fast forward 약자.
--no--ff로 merge 시 이전 커밋 목록들을 병합하는 commit이 기록 된다.
squash는 '뭉개다'라는 뜻. 이전 커밋들을 반영하지 않고 새로운 커밋 하나로만 반영된다.
rebase는 모든 커밋이 합쳐지지 않고 각각 추가된다.

일회성 브랜치에는 squash, 지속적인 브랜치에는 merge --no--ff를 활용하는 것이 좋다.