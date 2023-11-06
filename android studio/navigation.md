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
```