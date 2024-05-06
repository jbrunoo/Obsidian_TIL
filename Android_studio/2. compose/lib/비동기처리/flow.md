#### basic
	compose는 state에 의해 ui를 그리므로 상태를 보존하기 위한 mutableState<T>를 지원하고 remember로 상태를 기억.
	ui 관련된 상태 값은 mutableState를 쓰나 데이터와 같은 state는 최근 livedata, flow를 쓰는 추세.
	why?
	크게 4가지. 단일 출처로 유지 가능 / 비동기 작업 처리 / 데이터 흐름의 일관성 / 성능 및 메모리 관리
	compose는 관찰 가능한 유형에서 state<T>로 만드는 함수가 내장되어 있음
	특히, flow에서는 collectAsState() -> collectAsStateWithLifecycle()로 수명주기를 인식하고 앱 리소스를 절약할 수 있음


- - -
[kotlin flows in practice](https://www.youtube.com/watch?v=fSB6_KE95bU&t=75s)
[kotlin blog](https://medium.com/hongbeomi-dev/%EC%A0%95%EB%A6%AC-%EC%BD%94%ED%8B%80%EB%A6%B0-flow-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-android-dev-summit-2021-3606429f3c5f)
영상에서 보듯, 데이터를 직접 확인하고 가져오는 작업이 아닌 파이프라인을 설치하여 데이터를 자동으로 수집하고 수도꼭지를 열고 닫음으로써 조절함. 즉, 데이터를 관찰하는 reactive programming.
코루틴의 suspend 함수로 이루어져 코루틴과 함께 비동기처리에 용이함.

android에서 데이터 소스나 레포지토리가 생산자, ui가 소비자 역할
dataStore, room, retrofit, workManager 등 데이터 소스 라이브러리는 대부분 Flow와 통합되어 있음 
(직접 만드려면 flow builder 활용)

flow는 데이터 스트림의 일종이고 구성은 producer - intermediary - consumer로 이루어진다.
producer
	flow {  } 블록 내부에서 emit을 통해 데이터를 생성
	데이터 소스 라이브러리는 Flow 통합되어 flow로 데이터 감싸서 받으면 됨
	datasource
intermediary
	생성된 데이터를 수정(map(변형), filter(필터링), onEach(연산) 등) - 중간 연산자()
	repository
consumer
	collect를 통해 생성된 데이터를 소비 - terminal 연산자()
	ui(or viewmodel)
	ps. 생성된 데이터를 viewmodel에서 상태홀더 클래스인 stateFlow를 통해 보유


위처럼 필요에 따라 생성되고 관찰되는 중에만 데이터를 전송하는 flow를 cold flow라고 함

flow 수집 시 고려사항 -> 백그라운드, 구성 변경
stateFlow는 물탱크 역할.
flow를 stateFlow 변환 시 .stateIn 연산자 함께 사용 가능
구성 변경 시에는 flow가 끊기지 않게, 백그라운드에서는 flow가 끊기게 한다면?
-> .stateIn 연산자에 started 매개변수는 WhileSubscribed(5000)처럼 시간 초과를 사용하여 flow 유지를 관리

- - -


###### [mutableState vs mutableStateFlow](https://stackoverflow.com/questions/70217780/mutablestate-vs-mutablestateflow)
[state vs stateFlow](https://www.youtube.com/watch?v=T8vApYJlW8o)
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
[flow와 suspend 함수](https://stackoverflow.com/questions/76030366/when-to-use-suspend-function-and-flow-together-or-seperate-in-kotlin)

[collect와 luanchIn](https://handstandsam.com/2021/02/19/the-best-way-to-collect-a-flow-in-kotlin-launchin/)

- - -

```kotlin
override fun fetchRecentEventsFlow(endIndex: Int): Flow<Resource<List<RecentEventImage>>> {
        return eventRemoteDataSource.fetchRecentEventsFlow(endIndex = endIndex)
            .map { eventResponse ->
                Resource.Loading<List<RecentEventImage>>()
                Timber.d("Loading")
                val result = eventResponse.culturalEventInfo.result
                val row = eventResponse.culturalEventInfo.row

                if (result.code == "INFO-000") {
                    Timber.d("SUCCESS")
                    val eventImages = row.map { it.toRecentEventImage() }
                    Resource.Success(eventImages)
                } else {
                    handleResult(result = result)
                }
            }
    }
```

DataSource에서 flow를 반환하다보니 .map 연산자를 통해 Resource로 감싸고 dto -> entity로 변경해주는 작업
Resource.Loading의 타입을 IDE에서 읽지 못하는 오류 및 Success만 수행됨. emit으로 특정 값을 방출하는 것이 아니기 때문에 전체를 수행한 것 같음

```kotlin
return flow {
            emit(Resource.Loading())
            eventRemoteDataSource.fetchEventsByCodeNameFlow(codeName = codeName, title = title)
                .map { eventResponse ->
                    val result = eventResponse.culturalEventInfo.result
                    val row = eventResponse.culturalEventInfo.row

                    if (result.code == "INFO-000") {
                        Timber.d("SUCCESS")
                        val events = row.map { it.toEvent() }
                        Resource.Success(events)
                    } else {
                        handleResult(result = result)
                    }
                }.collect {
                    emit(it)
                }
        }

```

flow를 빌드한 후 emit 해주었음. 하지만 이미 flow 값을 가져왔다보니 map한 결과를 다시 collect 해야하는(이중 flow) 번거로움 및 repository에서 flow를 소비해도 되는가에 대한 의문이 듦.

```kotlin
return eventRemoteDataSource.fetchRecentEventsFlow(endIndex = endIndex)
            .transform { eventResponse ->
                emit(Resource.Loading())
                Timber.d("Loading")
                val result = eventResponse.culturalEventInfo.result
                val row = eventResponse.culturalEventInfo.row

                if (result.code == "INFO-000") {
                    Timber.d("SUCCESS")
                    val eventImages = row.map { it.toRecentEventImage() }
                    emit(Resource.Success(eventImages))
                } else {
                    emit(handleResult(result = result))
                }
            }
```

transform이란 새로운 연산자를 찾았음. 이미 만들어진 flow를 바꾸는 연산자로 새로운 emit을 만들어줌

근데 내부 로직이 위의 처리와 같긴 함.

```kotlin
public inline fun <T, R> Flow<T>.transform(
    @BuilderInference crossinline transform: suspend FlowCollector<R>.(value: T) -> Unit
): Flow<R> = flow { // Note: safe flow is used here, because collector is exposed to transform on each operation
    collect { value ->
        // kludge, without it Unit will be returned and TCE won't kick in, KT-28938
        return@collect transform(value)
    }
}
```

map의 내부 로직도 transform 후 emit 
```kotlin
public inline fun <T, R> Flow<T>.map(crossinline transform: suspend (value: T) -> R): Flow<R> = transform { value ->
    return@transform emit(transform(value))
}
```

ps. return@ 부분은 람다식에서 return 하기 위함.