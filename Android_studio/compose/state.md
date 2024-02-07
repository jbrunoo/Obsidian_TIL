[공식문서](https://developer.android.com/jetpack/compose/state?hl=ko#state-hoisting)

- `val mutableState = remember { mutableStateOf(default) }`
- `var value by remember { mutableStateOf(default) }`
- `val (value, setValue) = remember { mutableStateOf(default) }` - 구조분해

by는 getValue, setValue import해야 함.


상태에 대한 이해.
[A Compose state of mind](https://www.youtube.com/watch?v=rmv2ug-wW4U)


[blog](https://tigeroakes.com/posts/mutablestateof-list-vs-mutablestatelistof/)
mutableStateOf(listOf<T>()) VS mutableStateListOf<T>()
