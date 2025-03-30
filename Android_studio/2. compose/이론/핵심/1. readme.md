기존 Support Library(android.support.\*) -> Jetpack(andriodx.\*)

Jetpack 라이브러리는 안드로이드 개발을 위한 다양한 도구들 지원, 그중 UI 작업을 위한 새로운 패러다임
xml -> compose(선언형)

compose 간략 요약
	화면을 그리는 작업 - composition
	화면을 그리는 작업이 실행되는 곳 - composable 함수 내(UI 요소 box, column, row 등)
	화면의 상태를 관리하는 값 - state(스낵바, 게시물, 댓글, 애니메이션, 스티커 등)
	state를 바꾸는 트리거가 되는 행위 - event(버튼 클릭 등)
	state의 변경에 따른 화면의 다시 그리는 작업 - recomposition
	composable 밖에서 화면 상태를 변경 시키는 것 - side effect(스낵바 표시, 다른 화면 이동 등)

ps. state의 변경을 알아차리는 방법 - snapShot 시스템
