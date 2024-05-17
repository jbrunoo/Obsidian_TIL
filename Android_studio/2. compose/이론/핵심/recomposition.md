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

