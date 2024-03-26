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
ps. 물론 내 코드에서 저렇게 쓰지 않고 다른 방법은 있을 듯

