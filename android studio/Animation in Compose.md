[유튜브 참고](https://www.youtube.com/watch?v=HNSKJIQtb4c)


이 세 가지의 질문에 대해 생각하면 애니메이션 분해를 쉽게 할 수 있음.

### 1. what property am i animating?
### 2. When should this property change?
### 3. How should this property animate?


### 1. WHAT PROPERTY? - 무슨 속성을 animating 할지
	Most animations are made up of changing different properties or values of a composable.
	- Scale(크기)
	- Translation(이동)
	- Rotation(회전)
	- Alpha(투명도)
	- Color(색상)

```Kotlin
modifier.graphicsLayer {this.translationX = if(drawerState == drawerState.open) drawerWidth else 0f
this.scaleX = if(drawerState == drawerState.open) 0.8f else 1f
this.scaleY = ~~~
val corners = ~~
this.shape = RoundedCornerShape(roundedCorners)}

```

-> Runs in the draw phase / Compose의 그리기 단계에서 적용.
즉, 여기에서 animation을 변경해도 모든 단계를 거칠 필요가 없으므로 재구성이 트리거 되지 않음.


### 2. WHEN? - animation이 발생해야 하는 시점과 트리거하는 시점
	일반적인 트리거 시점
	- On State Change(상태 변경 시) 
	- On Launch(컴포저블 실행 시)
	- Infinitely(무한히)
	- With Gesture(동작 시)

- On State Change
	viewModel이나 사용자가 취하는 작업(버튼 클릭, 스위치 토글 등)
	이러한 종류의 animation은 AsState API 사용 가능.
	animateColorAsState() / animateDpAsState() / animateFloatAsState() / animateSizeAsState() / animateRectAsState()
	시간에 따른 가치변화를 저장하고 관리. 일반적으로 일회성 상태 변경에 사용.

```Kotlin
val clicked by remember { mutableStateOf(false) }
val offset by animateIntOffsetAsState(
	targetValue = if (clicked) {
		IntOffset(pxToMove, pxToMove)
	} else {
		IntOffset.Zero
	}
)
Box(
	modifier = Modifier
		.offset{
			offset
		}
)
```

- Transition(translation 아님) - 전환 객체 사용. 
	상태에 따라 동시에 발생하는 조정된 상태 업데이트 허용.

```Kotlin
var currentState by remember { mutableStateOf(BoxState.Collapsed) }
val transition = updateTransition(currentState, label = "transition")

val rect by transition.animateRect(label = "rect") {state ->
	when(state) {
		BoxState.Collapsed -> Rect(0f, 0f, 100f, 100f)
		BoxState.Expanded -> Rect(100f, 100f, 300f, 300f)
	}
}

val rotate by transition.animateFloat(label = "rotate") {state ->
	when (state) {
	BoxState.Collapsed -> 0f
	BoxState.Extanded -> 45f
	}
}
```

- On Launch
	컴포저블이 표시될 때 실행. 일반적으로 화면을 시작할 때 발생
	ex) 실제 콘텐츠가 표시되기 전에 표시해야 하는 자리 표시자가 있는 경우.
	컴포저블 실행 시 animation을 표시하려면 animatable 사용하면 됨.

```Kotlin
val color = remember { Animatable(Color.Gray) }
LaunchedEffect(open) {
	color.animateTo(if (open) Color.Green else Color.Red)
}
Box(Modifier.fillMaxSize().background(color.value))
```

- Infinitely
	콘텐츠에 무한히 또는 영원히 animation 적용.

```Kotlin
val infiniteTransition = rememberInfiniteTransition()
val scale by infiniteTransition.animateFloat(
	infiniteValue = 1f, targetValue = 8f,
	animationSpec = infiniteRepeatable(tween(1000), RepeatMode.Reverse)
)
Text(
	text = "Hello",
	modifier = Modifier
		.graphicsLayer {
			scaleX = scale
			scaleY = scale
			transformOrigin = TransformOrigin.Center
		},
		style = LocalTextStyle.current.copy(
			textMotion = TextMotion.Animated)
)
```

- With Gesture
	제스처의 일부.
	ex) 사용자가 시트를 맨 위로 드래그하면 크기에 따라 변환이 수행되어야 함.
	이 패턴은 다른 구성요소에도 적용될 수 있음.
	- 터치가 시작될 때 중지
	- 제스처가 완료된 후에도 특정 위치로 사라지거나 맞춰지는 animation을 계속해야함. 
	- 그리고 자연스럽게 정지되는 느낌을 주어야 함.
		Drag gesture handling

1번.
```Kotlin
Box {
	var drawerState by remember {
		mutableStateOf(DrawerState.Closed)
	}
}

HomeScreenDrawer()
ScreenContents(
	modifier = Modifier
		.graphicsLayer {
			this.translationX =
				if (drawerState == DrawerState.Open)
					drawerWidth else 0f
			this.scaleX = if (drawerState == DrawerState.Open)
				0.8f else 1f
			this.scaleY = if (drawerState == DrawerState.Open)
				0.8f else 1f
		}
)
```

2번.
// 제스처를 생성하기 위해 animation 가능한 객체인 transitionX 도입.
// 이는 drawer 열린, 닫힌 상태 사이를 변환하는데 사용할 수 있는 transitionX 저장. 
```Kotlin
Box {
	var drawerState by remember {
		mutableStateOf(DrawerState.Closed)
	}
	val translationX = remember {
		Animatable(0f)
	}
	translationX.updateBounds(0f, drawerWidth) // animation 임계값 설정. 이 지점 이상 진행 x
	// 만약 어떤 지점에서 animation을 초과하려면 
	// 이 매커니즘 대신 snapTo로 변환할 때 임계값을 확인해야 함.
	
	// drag 가능 개념. Delta가 변경되면 animation이 변경된 drag양에 맞춰지는지 
	val draggableState = rememberDraggableState(onDelta = { dragAmount ->
		coroutineScope.launch {
			// 사용자가 화면의 제스처와 상호작용할 때 animation 지연을 원하지 않기 때문에 snapTo 사용
			translationX.snapTo(translationX.value + dragAmount)
		}})
}

HomeScreenDrawer()
ScreenContents(
	modifier = Modifier
		.graphicsLayer {
			this.transitionX =
				transitionX.value
			// 1과 0.8사이를 선형적으로 보간.
			// Lerp는 1과 0.8 숫자 사이의 분수가 어딨는지 계산하는 방법.
			// ex) 1, 0.8에 0.5 분수를 주면 0.9를 계산함.
			val scale = lerp(1f, 0.8f, transitionX.value / drawerWidth)
			this.scaleX = scale
			this.scaleY = scale
		}
		.draggable(draggableState, Orientation.Horizontal)
)
```



절반 들었음. 정리해야함. 뒷부분. 12:00 분 부터 fling gesture handling
### 3. HOW?