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

StateFul vs StateLess
내부에서 state를 관리하는지 아닌지 차이.
stateless를 만들기 위해 state hoisting 패턴을 사용.
호출자에게 state를 올림.
