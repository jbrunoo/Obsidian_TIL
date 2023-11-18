[공식문서](https://developer.android.com/jetpack/compose/state?hl=ko#state-hoisting)

- `val mutableState = remember { mutableStateOf(default) }`
- `var value by remember { mutableStateOf(default) }`
- `val (value, setValue) = remember { mutableStateOf(default) }` - 구조분해

by는 getValue, setValue import해야 함.

