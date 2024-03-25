### `context` 
안드로이드 애플리케이션에서 액티비티(Activity), 서비스(Service), 어플리케이션(Application) 등과 같은 안드로이드 컴포넌트들과 상호작용하기 위한 핵심 클래스(그래서 위처럼 this로 받음)

안드로이드 시스템 서비스에 접근하고, 리소스에 접근하며, 액티비티를 시작하고, 앱의 기본 설정을 가져오는 등 다양한 기능 제공

구체적 특징
1. **리소스 접근**: 리소스 파일(레이아웃, 문자열, 이미지 등)에 접근하기 위해 `Context`를 사용할 수 있습니다.

2. **시스템 서비스 호출**: 안드로이드 시스템 서비스에 접근하기 위해 `getSystemService()` 메서드를 사용할 수 있습니다. 예를 들어, `LocationManager`, `NotificationManager`, `LayoutInflater` 등을 얻어올 수 있습니다.
   
3. **액티비티 시작**: 다른 액티비티를 시작하기 위해 `startActivity()` 메서드를 호출할 때 `Context`를 사용합니다.
   
4. **패키지 정보 가져오기**: 패키지 이름, 버전 등의 정보를 얻어오기 위해 `PackageManager`를 통해 `Context`에 접근할 수 있습니다.


### `databinding`
기존 xml 기반 UI 개발 방식은 디자인과 로직이 나뉘어서 Data Binding 라이브러리 사용했으나
Compose에서는 필요없다. databinding은 compose에 state, recomposition, remember, mutableStateOf의 기능과 대응된다.

`then` : Modifier.then() 으로 순서를 유지하며 여러 수정자들을 함께 연결 가능. [참고](https://developermemos.com/posts/modifier-then-jetpack-compose)
예시코드(modifier를 받아야 함)
```kotlin
@Composable
fun MyComposable(
    modifier: Modifier = Modifier,
    content: @Composable BoxScope.() -> Unit,
) {
    OtherComposable(
        modifier = Modifier
            .fillMaxWidth()
            .then(modifier),
        content = content,
    )
}
```

내 코드(padding을 2개 주고 싶음)
```kotlin
fun mScaffold(content: @Composable (PaddingValues) -> Unit) {  
    Scaffold(  
        topBar = {  
            TopAppBar(  
                colors = TopAppBarDefaults.smallTopAppBarColors(containerColor = Color.Blue),  
                title = {  
                    Text(  
                        text = "I & Ground",  
                        modifier = Modifier.fillMaxWidth(),  
                        textAlign = TextAlign.Center,  
                        color = Color.White,  
                    )  
                },  
            )  
        }, content = { paddingValues ->  
            content(paddingValues)  
        }  
    )  
}

Row(  
    modifier = Modifier  
        .padding(horizontal = 16.dp)  
        .then(Modifier.padding(it))   // 이런식으로 또는 container를 하나 더 만들기?
) {}
```
ps. 물론 내 코드에서 저렇게 쓰지 않고 다른 방법은 있을 듯.


### 상태 표시줄 숨기기(완전한 full screen)
[1번](https://developer.android.com/training/system-ui/status?hl=ko)
- android 4.0(API 수준 14) 이하는 manifest 수정 or 프로그래매틱한 방법으로
- Android 4.1(API 수준 16) 이상에서는 `setSystemUiVisibility()`를 사용
	사용해 봤는데 deprecated한 부분들 존재.
	
[2번](https://developer.android.com/training/system-ui/immersive?hl=ko) 
 레거시.
 
[3번](https://developer.android.com/jetpack/compose/layouts/insets?hl=ko)
	windowInsets keyword 활용.

[compose 창 인셋](https://developer.android.com/jetpack/compose/layouts/insets?hl=ko)

[#1 stackoverflow](https://stackoverflow.com/questions/69688138/how-to-hide-navigationbar-and-statusbar-in-jetpack-compose) - 상태표시줄 제거 [#2 stackoverflow](https://stackoverflow.com/questions/74429460/how-to-implement-transparent-status-bar-in-jetpack-compose-android) - 상태 표시줄 투명
```kotlin
val insetsController = WindowCompat.getInsetsController(window, window.decorView)

insetsController.apply {
    hide(WindowInsetsCompat.Type.statusBars())
    hide(WindowInsetsCompat.Type.navigationBars())
    systemBarsBehavior = WindowInsetsControllerCompat.BEHAVIOR_SHOW_TRANSIENT_BARS_BY_SWIPE
}
```

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {  
enableEdgeToEdge() // theme.kt view 부분 지우고 사용(상태바 투명)  
super.onCreate(savedInstanceState)
```
