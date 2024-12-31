[공식문서](https://developer.android.com/develop/ui/compose/compositionlocal?hl=ko)

Compose는 UI 트리를 구성하고 데이터 또한 함수의 매개변수로써 함께 아래로 흐른다.

UI에 필요한 값들(색상, 유형, 스타일 등)을 모두 파라미터로 전달하는 것은 번거로운 일이다.

`CompositionLocal`은 암시적으로 Composition을 통해 데이터를 전달하기 위한 도구이다.


1. compositionLocal 정의
	compositionLocalOf
	staticCompositionLocalOf
2. compositionLocalProvider 