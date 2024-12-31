기본적으로 Scaffold + BottomBar를 이용하여 구현한다.

이 방식은 상위에서 같은 BottomBar를 공유하기 때문에 화면 전환이 부드럽다.

하지만 2가지 문제가 있다. (compose로 학습하면서 전제 조건은 단일 activity를 쓰고있다는 가정이다.)

1. 상위 Scaffold 밑에 BottomBar를 사용하지 않는 화면이 있다면 어떻게 관리할 것인가?
	이 부분은 가볍게는 변수로 또는 AppState 내에서 navController와 함께 shouldShowBottomBar 등의 프로퍼티를 두어 이 bool 값의 분기에 따라 bottomBar를 표시할지 안할지 결정할 수 있다.

	물론 대부분의 경우 TopLevelDestinations 로 두는데 navGraph 속 세부 화면에서도 bottomBar가 필요한 경우가 있다면, 관리가 매우 복잡해질 것 같다.

	보통 navGraph의 관리를 위해 NavGraphBuilder를 확장시켜 TopLevelDestinations 하위의 화면들을 관리하는데, 위 bottomBar가 필요한 경우에 대비해보기 위해 NavHost를 여러 개 만드는 것을 시도해보았다.(당연히 권장되지 않는 방법이다.) 상위 Scaffold 에서는 자유롭지만 여러 개의 navController가 생기면서 관리 및 화면 전환이 매우 어려워지고 꼬이게 된다. 사실상 불가능 하다..


2. bottomBar를 공유하지 않는 화면으로 전환 시, padding 이슈
	Scaffold 내 innerPadding을 content에 넘겨주면 바텀 바가 없는 화면 전환 시, 바텀 바가 없는 부분에서 화면 전환이 일어남(즉, 전체화면에서 전환 되는 것이 아님) 이질감이 들 수 밖에 없다.
	innerPadding으로 일어난 문제인데, 이는 이 값을 넘겨주지 않고 경고를 @Suppress 처리하거나,
	필요한 부분만 .caculate~~ 하거나 Scaffold의 Modifier.systemBarPadding 등으로 처리할 수 있다.

	하지만 문제는 애니메이션을 적용할 경우이다. 상위에 Scaffold이기 때문에 한번에 전환되지 않는 애니메이션의 경우 애니메이션이 시작하거나 끝나기도 전에 바텀바가 사라지거나 먼저 나타나 버린다.
	[stackoverflow](https://stackoverflow.com/questions/76284055/how-to-correctly-use-bottombar-navigationbar-for-top-level-destination-screens) 이 상황과 똑같다고 할 수 있는데, 
	아무리봐도 해결 방안이 복잡한 방법만 떠올라서 감이 잘 안온다.
