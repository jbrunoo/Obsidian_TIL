[공식문서](https://developer.android.com/jetpack/compose/side-effects?hl=ko)

#### effect란?
	composable 함수 범위 '밖'에서 발생하는 앱 상태에 관한 변경 사항을 의미

	composable의 수명 주기와 속성(예측할 수 없는 recomposition, 다른 순서로 composable의 recomposition, 삭제할 수 있는 recomposition)을 고려하면 composable에는 부수 효과가 없는 것이 좋음

#### effect 관리
	하지만 스낵바 표시, 특정 상태 조건에 따라 다른 화면으로 이동 등 일회성 이벤트를 트리거할 때 부수 효과 필요
	이러한 작업은 컴포저블의 수명 주기를 인식하는 관리된 환경에서 호출해야 함
	즉, 앱 상태 변경해야 하는 경우 이러한 부수 효과가 예측 가능한 방식으로 실행되도록 effect API 사용





[medium](https://medium.com/@l2hyunwoo/%EB%B2%88%EC%97%AD-jetpack-compose-side-effects-in-details-a98ca403768)
#### effect API
	SideEffect
	LaunchedEffect
	DisposableEffect