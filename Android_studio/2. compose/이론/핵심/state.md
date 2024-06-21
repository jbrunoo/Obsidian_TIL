[공식문서](https://developer.android.com/jetpack/compose/state?hl=ko#state-hoisting)
- `val mutableState = remember { mutableStateOf(default) }`
- `var value by remember { mutableStateOf(default) }`
- `val (value, setValue) = remember { mutableStateOf(default) }` - 구조분해

by는 getValue, setValue import해야 함.

상태에 대한 이해.
[A Compose state of mind](https://www.youtube.com/watch?v=rmv2ug-wW4U)

[blog](https://tigeroakes.com/posts/mutablestateof-list-vs-mutablestatelistof/)
`mutableStateOf(listOf<T>()) VS mutableStateListOf<T>()`

깔끔한 설명
[blog2](https://developer88.tistory.com/entry/Android-Jetpack-Compose-UI-Part1-State)


- -- 
state 즉, 상태는 compose의 전부라고 해도 과언이 아니다.
state의 변화를 통해 recomposition이 일어나고 UI를 변경 시킨다.
state 변화를 어떻게 알아차리는가? -> snapShot 시스템을 이용

Stateful vs Stateless
내부에서 state를 관리하는지 아닌지 차이.
stateless를 만들기 위해 state hoisting 패턴을 사용.
호출자에게 state를 올림.


mutableStateOf vs mutableStateListOf [stackoverflow](https://stackoverflow.com/questions/75019326/what-is-the-difference-between-mutablestateof-and-mutablestatelistof)
전자는 객체 생성이며 by를 통해서 위임 패턴 활용. recomposition이 새 인스턴스를 할당할 때 발생
즉, 새 항목이 추가되었을 때 recomposition이 되기를 바라면 복사본을 만들어 복사된 목록을 할당해주어야 함

후자는 관찰 가능한 list가 생성됨. 수행하는 모든 작업에 의해 recomposition이 발생함.

- - -
그 많은 State를 어떻게 관리할지 [참고 블로그 #1](https://velog.io/@cksgodl/AndroidCompose-Compose%EC%97%90%EC%84%9C%EC%9D%98-result-Intent-%EB%B0%8F-State%EA%B4%80%EB%A6%AC) [참고 블로그 #2](https://8iggy.tistory.com/275)
블로그 설명처럼 
App 전반에 걸쳐진 navController, scaffoldState,
비즈니스 로직에 데이터들의 state, 화면 ui 로직의 state 값이 있다
비즈니스 로직의 state들은 구성 변경에도 살아남는 viewModel에
ui의 state 값들은 사용되는 composable 내부 또는 App 전반에 걸친 state를 모아 plain state holder class를 만드는 것이 좋다.
