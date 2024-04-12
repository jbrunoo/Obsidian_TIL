#### basic
	compose는 state에 의해 ui를 그리므로 상태를 보존하기 위한 mutableState<T>를 지원하고 remember로 상태를 기억.
	ui 관련된 상태 값은 mutableState를 쓰나 데이터와 같은 state는 최근 livedata, flow를 쓰는 추세.
	why?
	크게 4가지. 단일 출처로 유지 가능 / 비동기 작업 처리 / 데이터 흐름의 일관성 / 성능 및 메모리 관리
	compose는 관찰 가능한 유형에서 state<T>로 만드는 함수가 내장되어 있음
	특히, flow에서는 collectAsState() -> collectAsStateWithLifecycle()로 수명주기를 인식하고 앱 리소스를 절약할 수 있음


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





- - -
#### 위에 flow는 cold stream이라고 했는데 무엇인지? [medium](https://medium.com/@apfhdznzl/flow%EC%99%80-channel-cold-stream%EA%B3%BC-hot-stream-c42c64cf4996)
	observable, flow, channel 같은 자료구조는 생산자-소비자 패턴을 이용함.
	여기서 중요한 개념이 cold / hot stream

###### basic
	cold stream(Flow) vs hot stream(StateFlow, SharedFlow)
	1. 데이터가 생산되는 위치
	2. 생산자가 발행한 데이터를 동시에 여러 소비자들이 수신할 수 있는지 여부
	3. 스트림이 데이터를 생산하는 시점

###### 간편 이해
	cold stream = CD player
	CD player는 CD 내부에 음악 목록이 저장(데이터가 내부에서 생성)
	모든 사용자는 동일한 음악 목록을 CD에 저장하고 있지만 하나의 CD를 공유하지 않음(하나의 생산자에 하나의 소비자 존재)
	사용자 마다 재생 버튼 누르면 음악 재생(소비자가 소비를 시작할 때 데이터 생산)
	
	hot stream = Radio
	Radio는 방송국에서 프로그램 제작(데이터가 외부에서 생성)
	청취자들에게 동시에 송신(하나의 생산자에 다수의 소비자 존재)
	뒤늦게 라디오 주파수를 맞추면 청취 시점부터 들을 수 있음(생산자가 소비자의 소비를 신경쓰지 않고 생산)


- - -
[kotlin flows in practice](https://www.youtube.com/watch?v=fSB6_KE95bU&t=75s)

clean architecture에서 어느 layer or class에서 flow로 변환해야 하는지? (사람마다 다르게 구현해둠)
