[공식문서](https://developer.android.com/develop/ui/compose/phases?hl=ko)

[[recomposition]] 글에서 성능 최적화를 살펴보았다.
그중에 상태 지연 기법으로 람다로 값을 넘겨주는 방식이 있었는데
이해가 안된 부분은 다음과 같다.

uiState 전체를 넘겨주면 당연히 모든 composable에서 개별 요소가 변하면 변경 되겠지만
uiState.xx 개별 요소를 넘겨주면 개별 요소에 해당해서 지능적으로 compose가 recomposition 해준다.

그럼 `{ }로 값을 왜 넘겨줘야 하는가?` 에 대해서 상태 지연을 얘기한다.
처음 공부할 때는
`어짜피 화면에 나타나는 건 똑같은데 어디서 지연이 된다는 거지?`  생각했으나
위 공식문서의 컴포즈 단계를 살펴보면 아 단계 부분에서 지연되서 처리하는구나 이해가 된다.

xml에서는 측정 - 레이아웃 - 그리기 순서로 진행된다.
compose는 composition - layout -drawing 순이다.

compose의 layout에서는 기존 측정 + 레이아웃 작업을 진행한다.
composition에서는 composable 함수를 실행하고 UI Description(UI 트리)을 만든다고 한다.
우리의 상태(ex: uiState)가 변화하면 composition 부분이 다시 일어난다. 즉 recomposition.

{} 람다로 값을 넘기게 되면 composition 시점이 아닌 레이아웃 단계에서 일어나게 된다.

ps. 주의할 점은 composition이 일어나는 범위는 scope 내인데 @Composable 기준이다.
component의 {} 부분과 혼동하지 말아야 한다.

- - -
이유는 알았다. 언제 사용해야될지 얘기를 해볼까 한다.

모든 상태에서 사용할 필요는 없다. 왜냐하면 문서에 적혀있듯 상태가 변하면 UI 트리를 새로 그리는 것은 의도된 행동이기 때문이다. 문제인 부분은 상태가 빠르게 변하는 경우 그리고 그 state의 개별 값을 다른 컴포넌트에서 참조하고 있는 경우이다.

대표적으로 소개하는 예시는 scroll 할 때 offset 같은 경우이다.
특정 컴포넌트에서는 해당 offset 위치 값을 보여주는데 scroll의 state가 변할 때마다 함께 호출되는 경우이다.

나의 경우에도 시간을 밀리초로 바꾸어 표시하는 타이머 기능을 구현해보다가 모든 컴포저블이 recomposition을 발생하는 장면을 경험한 것이 학습을 시작했는데 해결은 했었지만 작동원리를 다시 이해하고 간다.