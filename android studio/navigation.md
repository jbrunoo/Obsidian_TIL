navController vs navHostController [참고](https://jgeun97.tistory.com/334)

즉 NavController를 직접사용할 수는 있지만, 하위 구현체는 NavHostController를 통해 액새스하는 것이 좋다고 설명합니다.

저희가 사용하는 대부분의 기능들은 NavController에 포함되어 있고 NavHostController 4가지 함수만 override하는 형태입니다.

- setLifecycleOwner
- setOnBackPressedDispatcher
- enabledOnBackPressed
- setViewModelStore

NavController는 내부에서 사용하기 위한 Navigation의 대부분 동작을 명시해놓은 class이며, 개발자가 이 코드에 직접적으로 접근하는 것을 방지하기 위해 NavHostController를 통해 접근하도록 한 것이라는 생각이 들었습니다.

Nested Navigation [참고](https://nameisjayant.medium.com/nested-navigation-in-jetpack-compose-597ecdc6eebb)
```kotlin
@Composable  
fun NestedNavigation() {  
  
	val navHostController = rememberNavController()  
	  
	NavHost(  
		navController = navHostController,  
		startDestination = NestedScreen.Splash.route  
	) {  
	  
		composable(NestedScreen.Splash.route) {  
			SplashScreen()  
		}  
		  
		navigation(  
			route = NestedScreen.Register.route,  
			startDestination = NestedScreen.Register.Login.route  
		){  
			composable(NestedScreen.Register.Login.route){  
				LoginScreen()  
			}  
			composable(NestedScreen.Register.Signup.route){  
				SignUpScreen()  
			}  
		}  
		  
		composable(NestedScreen.Dashboard.route){  
			DashboardScreen()  
		}  
	  
	}  
  
}
```

```kotlin
@Composable  
fun SplashScreen(  
	navHostController: NavHostController  
) {  
	Box(modifier = Modifier.fillMaxSize(), contentAlignment = Alignment.Center) {  
		Button(onClick = {  
			navHostController.navigate(NestedScreen.Register.route)  
		}) {  
			Text(text = "Go to register screen")  
		}  
	}  
}
```



stackoverflow 예시
```kotlin
```kotlin
In your Main Activity. Pass to the Navigator any parameters your screens will need such as viewModels.

val navController = rememberNavController()
Navigator(navController, toLogin: () -> Unit) // 콜백?

Create a file like below with an object for each screen you want.
sealed class Screens(val route: String) {
    object Page1: Screens("Page1")
    object Page2: Screens("Page2")
    object Page3: Screens("Page3")
    object Page4: Screens("Page4")
}

Create a composable as follows. The startDestination is what screen the app will open to when started.
@Composable
fun Navigator (navController: NavHostController,
              ) {
    NavHost(
        navController = navController,
        startDestination = Screens.Page1.route)
    {
        composable(route = Screens.Page1.route){ 
            Page1(viewModel, navController) // viewModel을 넘겨주네
        }
        composable(route = Screens.Page2.route){
            Page2(viewModel, navController)
        }
        composable(route = Screens.Page3.route){
            Page3(viewModel, navController)
        }
        composable(route = Screens.Page4.route){
            Page4(viewModel, navController)
        }
    }
}

From any of your composables you can navigate to another with this.
navController.navigate(Screens.Page3.route)
```
`

## 데이터 값 넘기기 [블로그 참고함](https://velog.io/@zinkiki/AndroidCompose-navigation%EB%84%A4%EB%B9%84%EA%B2%8C%EC%9D%B4%EC%85%98-%EA%B5%AC%ED%98%84)

기본 자료형과 참조 자료형 넘기는 방식 다름
```Kotlin
```kotlin
// 기본자료형 전달
        composable(NAV_ROUTE.PAGE1.routeName/{data}", arguments = listOf(navArgument("data"){type = NavType.IntType})){
        	Page2(data)
        }
        
        // 참조자료형 전달
        composable(NAV_ROUTE.PAGE2.routeName){
        	val data = remember {
                navController.previousBackStackEntry?.savedStateHandle?.get<TestData>("data")
            }
        	Page3(data)
        }
```


### [여러 값 넘기기](https://workspace-dev.medium.com/navigation-compose%EC%97%90%EC%84%9C-argument%EB%A1%9C-bundle%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-beb119d5500b)
1. bundle 사용.
	`@Parcelize`는 Android에서 Kotlin 데이터 클래스에 대한 Parcelable 구현을 자동으로 생성하기 위한 어노테이션입니다. Parcelable은 안드로이드에서 객체를 전달할 때 사용되는 인터페이스로, 데이터 클래스를 Parcelable로 만들면 Intent나 Bundle 등을 통해 객체를 손쉽게 전달할 수 있게 됩니다.

[공식문서](https://developer.android.com/guide/navigation/navigation-pass-data?hl=ko) -> compose는 아님.
경로, 딥 링크 및 인수가 포함된 URI를 문자열에서 파싱할 수 있습니다. 위의 표에서 보는 바와 같이 Parcelable 및 Serializable과 같은 맞춤 데이터 유형은 사용할 수 없습니다. 맞춤 복합 데이터를 전달하려면 ViewModel 또는 데이터베이스와 같은 다른 위치에 데이터를 저장하고 탐색 중에 식별자만 전달한 다음 탐색이 완료된 후 새로운 위치에서 데이터를 회수합니다.

**주의:** 인수를 통해 복잡한 데이터 구조를 전달하는 것은 안티패턴으로 간주됩니다. 각 대상은 항목 ID와 같이 필요한 최소 정보를 기반으로 UI 데이터 로드를 담당해야 합니다. 이를 통해 프로세스 재생성이 간소화되고 잠재적 데이터 불일치가 방지됩니다.


앞으로는 값을 넘기는거보다 db에 저장하고 빼서 쓰는 방식으로 구현해야 될듯하다.

[공식문서 navigation in compose](https://developer.android.com/jetpack/compose/navigation?hl=ko)



예전에 강사님이 적어줬던 코드
```
NavHost( navController = navController, startDestination = MainDestination.InputScreen.route ) { composable(MainDestination.InputScreen.route) { InputInfo(navController) } composable(MainDestination.UserScreen.route) { backStackEntry -> UserInfo( navController = navController, name = (backStackEntry.arguments?.getString("name") ?: "") ) } }

// sealed class안에서 함수
sealed class MainDestination(val route: String) { object InputScreen : MainDestination("inputInfo") { fun actionToUserScreen(name: String): String { return when { name.isEmpty() -> throw Exception("check name is not empty") else -> "userInfo/$name" } } } object UserScreen : MainDestination("userInfo/{name}") }
```

ps. [why use sealed class?](https://stackoverflow.com/questions/69686087/why-use-sealed-class-and-make-object-in-navigation-kotlin-jetpack-compose) 



- bottom navigation bar
[blog](https://velog.io/@chuu1019/Android-Jetpack-Compose-Bottom-Navigation-%EB%A7%8C%EB%93%A4%EA%B8%B0)
material은 BottomNavigation
material3는 BottomAppBar

```kotlin
@Composable  
fun BottomNavigationBar(navController: NavHostController) {  
//이동 경로 items 설정
    val items = listOf(  
        BottomNavItem.Friends,  
        BottomNavItem.Chat,  
        BottomNavItem.Shop,  
        BottomNavItem.Others  
    )  
    BottomAppBar(  
        containerColor = Color(0xFFE0E0E0),  
        contentColor = Color.Black  
    ) {  
    // 현재 route 가져오기
        val navBackStackEntry by navController.currentBackStackEntryAsState()  
        val currentDestination = navBackStackEntry?.destination?.route  
        items.forEach { screen ->  
            BottomNavigationItem(  
                icon = { Icon(screen.icon, contentDescription = screen.title) },  
                label = { Text(screen.title, fontSize = 12.sp) },  
                selected = currentDestination == screen.route,
                selectedContentColor = Color.Yellow,  
                onClick = {  
                    navController.navigate(screen.route) {  
                    // 상태 저장을 통해 같은 icon은 이전에 저장한 값으로 recomsition 방지
                        popUpTo(navController.graph.startDestinationId) { saveState = true }  
                        launchSingleTop = true  
                        restoreState = true  
                    }  
                }            )  
        }  
    }}
```