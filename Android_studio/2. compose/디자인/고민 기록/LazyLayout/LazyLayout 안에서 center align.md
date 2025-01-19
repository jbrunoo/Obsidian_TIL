기본적으로 lazyLayout의 크기는 측정할 수 없기에,
가장 간편한 해결책은 lazyLayout을 측정 가능한 component로 감싸준다.

```kotlin
Box(
	modifier = Modifier.fillMaxSize,
	contentAlignment = if(paginationStatus == PaginationStatus.EMPTY) Alignment.Center else Alignment.TopStart
) {
	LazyColumn {
		when(paginationStatus) {
			PaginationStatus.EMPTY -> Text("center")
			else -> Text("topStart")
		}
	}
}
```
