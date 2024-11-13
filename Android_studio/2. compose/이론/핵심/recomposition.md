#### compose 모범 사례
[공식문서](https://developer.android.com/develop/ui/compose/performance/bestpractices?hl=ko) 

결국 compose는 recomposition을 줄이는 것이 핵심.
반복되는 계산을 줄이고 recomposition 범위를 줄이고 등등..

remember로 비용 많이 드는 계산 최소화.
lazyLayout 활용 및 key 사용.
derivedStateOf 활용하기 (대표적인 예: 스크롤 화면에서 새 항목이 로드되는 state에 도달할 때 재구성하기 위함)
역방향 쓰기 방지  - 이미 상태를 읽은 후에 다시 상태를 쓰는 행위. 무한루프에 빠질 수 있음.
가능한 상태를 늦게 읽기 - 아래 참고 ↓
- - - 
#### donut hole skipping과 recomposition
[기사](https://www.jetpackcompose.app/articles/donut-hole-skipping-in-jetpack-compose)

recomposition이 일어나는 방식이 도넛 안에 도넛 안에 도넛 안에.. 형태와 유사
정확히는 composable 함수는 도넛, {} 이루어진 범위는 도넛 구멍.

recomposition 횟수는 성능과 직접적인 연관이 있으므로 어떻게 처리하는 것이 좋을지 ?

compose runtime은 기본적으로 최소한의 recomposition의 범위를 찾으려고 최적화 되어있음.
Android의 UDF를 따르고 테스트, 캡슐화 등을 위해 stateless를 추구하는데 state hoisting을 하면 state 변화에 따른 하위 composable에 단순 parameter로 넘겨주게 되면 상위 state 변화에 따라 모든 하위 컴포저블이 재구성됨.

즉, stateless한 하위 컴포저블에서만 사용하는 state라면 함수나 람다에서 state를 읽어야 그 부분만 재구성됨.
[가능한 상태를 늦게 읽기](https://developer.android.com/develop/ui/compose/performance/bestpractices#defer-reads) 를 통해서 예제를 보면,
```kotlin
@Composable
fun SnackDetail() {
    // ...
    Box(Modifier.fillMaxSize()) { // Recomposition Scope Start
        val scroll = rememberScrollState(0)
        // ...
        Title(snack, scroll.value)
        // ...
    } // Recomposition Scope End
}

@Composable
private fun Title(snack: Snack, scroll: Int) {
    // ...
    val offset = with(LocalDensity.current) { scroll.toDp() }
    Column(
        modifier = Modifier
            .offset(y = offset)
    ) {
        // ...
    }
}
```

scroll.value를 람다식으로 넘겨줌으로써 scroll 값 변화에 따라서 Title composable만 재구성 됨.

```kotlin
@Composable
fun SnackDetail() {
    // ...
    Box(Modifier.fillMaxSize()) { // Recomposition Scope Start
        val scroll = rememberScrollState(0)
        // ...
        Title(snack) { scroll.value } // 람다식으로 넘겨줌
        // ...
    } // Recomposition Scope End
}

@Composable
private fun Title(snack: Snack, scrollProvider: () -> Int // 함수로 받음) {
    // ...
    val offset = with(LocalDensity.current) { scrollProvider().toDp() }
    Column(
        modifier = Modifier
            .offset(y = offset)
    ) {
        // ...
    }
}
```

여기서 추가적으로 성능을 개선한다면, offset까지 수정자의 람다로 넘겨줌.

```kotlin
@Composable
private fun Title(snack: Snack, scrollProvider: () -> Int) {
    // ...
    Column(
        modifier = Modifier
            .offset { IntOffset(x = 0, y = scrollProvider()) }
    ) {
        // ...
    }
}
```

**다른 예시**
두 가지 색상을 변경하는 color 값을 box의 배경색으로 가지면 box의 content는 계속 재구성될 것임.

```kotlin
// Here, assume animateColorBetween() is a function that swaps between
// two colors
val color by animateColorBetween(Color.Cyan, Color.Magenta)

Box(
    Modifier
        .fillMaxSize()
        .background(color)
)
```

drawBehind {} 같은 람다 기반 수정자를 이용하여 그리기 단계에서만 값을 활용. composition, layout 단계 건너뜀.

```kotlin
val color by animateColorBetween(Color.Cyan, Color.Magenta)
Box(
    Modifier
        .fillMaxSize()
        .drawBehind {
            drawRect(color)
        }
)
```

ps. UI에 관한 상태라면 compositionLocal을 이용할 수도 있음(찾아본 후 다음에 후술 계속..)

- - - 
내 코드에서 적용해보았음

viewModel에 카운트 다운을 밀리초 단위로 하는 함수 및 변수가 있었음 
```kotlin
/* viewModel에서 _limitTime 변수를 countDown 해줌 */
private val _limitTime = MutableStateFlow(questionCount * 5000L) // milliseconds  
val limitTime = _limitTime.asStateFlow()

private fun countDownTime() {
	viewModelScope.launch(Dispatchers.IO) {
		while(_limitTime.value > 0L) {
			delay(10L)
			_limitTime.update { it - 10L }
			// _limitTime.value -= 10L // 결과는 똑같음
		}
	}
}
```

limitTime이 밀리초 단위로 모든 composable을 recomposition함. 다른 composable에서도 로그 찍어봄.
```kotlin
@Composable
fun PlayScreen(
    navController: NavHostController,
    viewModel: PlayViewModel = hiltViewModel()
) {
    Log.d("playscreen 계속 recomposition?", "playscreen 계속 recomposition?")
    val uiState = viewModel.uiState.collectAsStateWithLifecycle()
    val limitTime = viewModel.limitTime.collectAsStateWithLifecycle()

    when (val state = uiState.value) {
        is PlayUiState.LOADING -> CircularProgressIndicator()
        is PlayUiState.SUCCESS -> {
            var showResultDialog by remember { mutableStateOf(false) }
            if(limitTime.value < 0L) showResultDialog = true
            Column {
                TimerLayout(
                    limitTime = limitTime.value,
                )
                ContentCarousel(
                    // ...
                )
            }
            if (showResultDialog) {
                // ...
            }
        }
    }
}
```

limitTime 즉, state를 가지는 두 곳(아래 코드 참고)에서 recomposition을 일으키는데
donuthole skipping에 따라 가장 인근의 composable scope를 찾게 됨.
if문에 쓰인 limitTime은 PlayScreen 전체를 recomposition

~~if문이 없었더라도
TimerLayout 안의 limitTime은 Column을 recomposition
즉, ContentCarousel도 recomposition 됨~~
=> 정정 Column, Row, Box 등 기초 component들은 실제로 인라인 함수이며 restart하는 범위가 아님.
=> 즉, TimeLayout을 가지고 있는 playscreen이 영향 받음.
```kotlin
if(limitTime.value == 0L) showResultDialog = true

Column {
	TimerLayout(
		limitTime = limitTime.value,
	)
	ContentCarousel(
		// ...
	)
}
```

해결방법
```kotlin
// limitTime.value가 scope 안에서 실행되게 만들어 주었음
TimerLayout(
	limitTime = { limitTime.value },
	onTerminate = { showResultDialog = true }
)

// 또한 stateless 함
@Composable
fun TimerLayout(limitTime: () -> Long, onTerminate: () -> Unit) {
    val time = limitTime()
    val seconds = time / 1000
    val milliSeconds = (time % 1000) / 10
    if (time == 0L) onTerminate()
    Text(
        text = String.format("Timer: %d.%02d", seconds, milliSeconds),
        style = TextStyle(
            fontSize = 30.sp,
            fontWeight = FontWeight.ExtraBold
        )
    )
}
```

밀리초 단위로 recomposition을 하기 때문에 적용해보긴했지만..
예시로 찾는 코드들이 대부분 stateless 하게는 만들었지만
recomposition에 대한 처리가 별로 없어서 이렇게 다 써야하는건지에 대해서는 의문이 있음

- - -
추가 읽어볼 자료

[참고 medium - compose의 안정성](https://medium.com/androiddevelopers/jetpack-compose-stability-explained-79c10db270c8)
원시 타입이 아닌 타입들은 컴파일러가 변경 불가능한지에 대한 확신이 없어서 recomposition이 일어남.
@Stable, @Immutable 또는 [persistentList 같은 kotlinx 불변 컬렉션](https://github.com/Kotlin/kotlinx.collections.immutable) 처리가 필요.

[stackoverflow](https://stackoverflow.com/questions/74032640/what-would-be-the-best-way-to-handle-state-in-compose-avoiding-recomposition-usi)
위 질문의 Option 1 방식처럼 uiState.param 으로 나누어 쓰는 것은 알고 있었지만 왜 그런지 아직 모름.

With Option 1, you are right that It will be recomposed if 1 parameters changed but since Compose can intelligently recompose only the components that changed, you would not have problem.
답변도 compose가 지능적으로 할 수 있다는 것이 .. 궁금하다
