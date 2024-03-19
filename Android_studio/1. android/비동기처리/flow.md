###### [mutableState vs mutableStateFlow](https://stackoverflow.com/questions/70217780/mutablestate-vs-mutablestateflow)
	state는 compose api로 ui 를 recomposition 하기 위한 목적
	
	flow는 코루틴 api로 본 목적은 코루틴의 비동기 스트림 api
	코루틴의 suspend가 하나의 값만 return하기 때문에 지속적 or 여러개의 값에 대응하기 위함
	emit()으로 stream으로 방출하고 이를 collect 하여 사용
	즉, ui 목적이 아니기 때문에 recomposition을 원한다면 collectAsState 활용

	stream -> cold / hot stream
	hot stream : 데이터를 소비하는 곳과 무관하게 아이템들을 생산(list, set같은 collection, channel)
	cold stream : 필요할 때 데이터를 생산(메모리 관리) -> lazy함 (flow, sequence)

	flow는 cold stream으로 즉 collect() 연산자가 있을 때 동작 (collect만 코루틴 블록에서 실행되면 됨)
	모든 api는 아니고 cold, hot 둘 다 존재

	upstream, downstream


[medium](https://medium.com/hongbeomi-dev/jetpack-compose%EC%97%90%EC%84%9C-flow%EB%A5%BC-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-a394a679909b)
collectAsState -> collectAsStateWithLifecycle 
액티비티 등의 생명주기를 따라 flow를 수집할지 결정함
onStop -> onStart 사이처럼 앱이 백그라운드에 있을 때 전자는 활성화되어있고 후자는 멈춘 상태
대부분의 경우 후자를 사용하는 것이 리소스 확보를 위해 유리함
전자는 android가 아닌 다른 플랫폼을 위해 개발할 때 사용되어야 함

