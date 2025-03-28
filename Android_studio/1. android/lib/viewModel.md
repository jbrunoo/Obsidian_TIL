
### 1번째 viewModel 고찰
ViewModelFactory는 ViewModel을 단일 객체로 생성해 사용하기 위해(= 싱글톤 패턴이라나) 구현하는 클래스


navigation과 viewModel
복잡한 객체는 단일 정보 소스(예: 데이터 영역)에 데이터로 저장해야 합니다. 탐색 후 대상에 도달하면 전달된 ID를 사용하여 단일 정보 소스에서 필요한 정보를 로드할 수 있습니다. 데이터 영역 액세스를 담당하는 ViewModel의 인수를 검색하려면 `ViewModel’s` [`SavedStateHandle`](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate?hl=ko#savedstatehandle)을 사용하면 됩니다.

[viewModel 공식 문서](https://developer.android.com/topic/libraries/architecture/viewmodel?hl=ko#kotlin_1)

kotlin xml 식
```
val viewModel: DiceRollViewModel by viewModels()
```
java 식
```
DiceRollViewModel model = new ViewModelProvider(this).get(DiceRollViewModel.class);
```
compose 식
``` 
viewModel: DiceRollViewModel = viewModel()
```


### 2번째 viewModel 고찰
단순히 누가 어떤 언어를 사용해서 개발했는지에 따라 위와 같이 차이점을 예전에 나누어 두었음..
다시 한 번 viewmodel에 대해 찾아보며 viewmodel을 3가지 의미로 나누어 보았다.

1. MVVM ViewModel
2. AAC ViewModel
3. Compose ViewModel

첫째, mvvm viewmodel은 말그대로 model view viewmodel 설계의 viewmodel을 의미한다.
즉, view의 로직, state 등을 분리시켜 재사용성, 테스트 용이 등의 이점을 보여주는 개념적인 의미.
더 정확히는 View와 Model 사이 데이터 관리 및 바인딩 역할.


둘째, 일단 AAC(Android Architecture Components)는 17년도에 발표된 라이브러리로 LifeCycle, LiveData(이걸 view에서 observe하던데 요즘은 flow로 구현하는 것 같음), ViewModel, Room, Paging, Databinding, Navigation, WorkManger 로 구성되어 있다고 한다.
각각의 개념들이 다 중요하지만 이 중에 ViewModel을 의미하는 것으로 AAC 뜻대로 안드로이드 아키텍쳐를 위한 컴포넌트이다. 이 viewModel이 화면 전환에도 데이터가 변하지 않는 즉, activity안에 LifecycleEventObserver()로 view의 LifeCycle을 관찰하다가 View의 종료와 함께 Clear() 호출.
즉, activity와 생명주기를 다르게 가져가기 때문에 앱 종료 전까지 계속 데이터를 유지할 수 있다.
[블로그](https://medium.com/kenneth-android/android-mvvm-viewmodel%EA%B3%BC-aac-viewmodel%EC%9D%98-%EC%B0%A8%EC%9D%B4-8c0d54922e07) 이 글에 따르면, mvvm 용으로 설계된 것은 아니고 activity 내에 1개만 생성가능하다는데 mvvm에서는 view와 viewmodel을 1:n || n:1 관계도 가능하다고 했음. 작성자는 그것이 1번과의 차이로 보는듯함.


셋째, Compose ViewModel은 개념적으로 다른 것은 없고 compose로만 배웠기 때문에 헷갈렸던 것인데 AAC ViewModel을 Compose용으로 만들었다고 생각하면 될 것 같다. 초기화 방식도 기존, viewModelProvider 같은 부분에서 간소화 되고 compose에 특화되지 않았을까 생각한다.



### 3번째 viewModel 고찰 (개발자님께 질문 예정)
[MVVM with Grab Architecture](https://tv.naver.com/v/4637223?playlistNo=272653)
[깃헙예제](https://github1s.com/skydoves/DisneyCompose/blob/main/app/src/main/java/com/skydoves/disneycompose/ui/main/MainActivity.kt)
[코드랩](https://developer.android.com/codelabs/jetpack-compose-advanced-state-side-effects?hl=ko&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fcompose%3Fhl%3Dko%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fjetpack-compose-advanced-state-side-effects#4)

1번째 강의를 듣고 많은 생각이 바뀜. 
일단 mvvm은 마이크로소프트에서 언급한 개념. 구글에서 viewModel과 함께 문서에는 mvvm을 찾아볼 수 없음. 즉 상태 홀더로써의 기능을 중시하는 것 같음.
구현할 수 있다고 여러 블로그에서 얘기하는데 강연자의 말에 따르면 viewmodel안에 livedata를 observe로 보는 코드 - 이렇게 짜면 결합성이 너무 높다. mvvm에서 벗어난다고 언급.

하지만 5년전 강의다 보니 xml과 databinding을 이용하고 지금 AAC viewModel에 바뀐 부분이 있는지 알 수 없었다. Compose는 어떤 식인지 궁금해서 skydoves님의 mvvm 예제 코드를 살펴보았음. 따로 compose에서 databinding의 역할을 하는 부분을 찾지 못함. 그래서 아마 AAC의 viewModel과 DI를 함께 쓰는 이유가 있지 않을까 생각해서 개발자님께 질문 예정.



#### viewModel backing properties
[블로그](https://everyday-develop-myself.tistory.com/m/344)
변수에서 살펴보았던 getter, setter, =, by와 함께

- - -
hilt 사용 없이 viewmodel에 저장소(repository, dao) 같은 종속 항목 주입 경우 viewmodel.factory DSL을 사용해야 함 [공식문서](https://developer.android.com/topic/libraries/architecture/viewmodel/viewmodel-factories?hl=ko#jetpack-compose)

[ui 상태 저장](https://developer.android.com/topic/libraries/architecture/saving-states?hl=ko)
compose에서 rememberSaveable, view에서 onSaveInstanceState() 있듯 
viewModel에서 SavedStateHandle 활용
viewmodel은 구성변경 시에는 유지되지만(부분적인 데이터 유지) 시스템에서 종료되거나 사용자의 완전한 종료 등에 의한 데이터를 기억하기 위함

- - -
viewModel 상태를 어떻게 관리할지 (feat. MVVM vs MVI)
single state vs multiple state  [medium](https://medium.com/proandroiddev/android-viewmodel-single-state-or-not-d914f962d44c)
single state는 sealed class로 state를 관리하고 그 state를 뷰모델에서 flow 등으로 불러서 .copy 이용하여 업데이트함. copy 메서드를 이용하기 때문에 불필요한 

- - -
[medium - Mastering Android ViewModels](https://proandroiddev.com/mastering-android-viewmodels-essential-dos-and-donts-part-1-%EF%B8%8F-bdf05287bca9)
1. Avoid initializing state in the `init {}` block.
2. Avoid exposing mutable states.
3. Use update{} when using MutableStateFlows:
4. Lazily inject dependencies in the constructor.
5. Embrace more reactive and less imperative coding.
6. Avoid initializing the ViewModel from the outside world.
7. Avoid passing parameters from the outside world.
8. Avoid hardcoding Coroutine Dispatchers.
9. Unit test your ViewModels.
10. Avoid exposing suspended functions.
11. Leverage the `onCleared()` callback in ViewModels.
12. Handle process death and configuration changes.
13. Inject UseCases, which call Repositories, which in turn call DataSources.
14. Only include domain objects in your ViewModels.
15. Leverage `shareIn()` and `stateIn()` operators to avoid hitting the upstream multiple times.


init 대신 변수 자체에서 flow emit {}.stateIn으로 데이터 로드 하기

- - -
savedStateHandle 사용 이유. 값 저장도 있고 navigation과 함께 인수 전달 가능
[참고 블로그](https://medium.com/@apfhdznzl/navigation-component%EC%97%90%EC%84%9C-fragment%EB%81%BC%EB%A6%AC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%A0%84%EB%8B%AC%ED%95%98%EA%B8%B0-61488354352b) [참고 블로그](https://developer88.tistory.com/entry/Route-%EC%97%90%EC%84%9C-%EB%84%98%EC%96%B4%EC%98%A8-%EA%B0%92%EC%9D%84-ViewModel-savedStateHandle-%EB%A1%9C-%EB%B0%9B%EB%8A%94-%EB%B0%A9%EB%B2%95-Jetpack-Compose-Navigaion)

지금까지는 navigation에서 navBackStackEntry에서 arguments로 넘겨줬는데
깊은 로직까진 모르겠지만 viewmodel savedStateHandle에 navBackStackEntry 정보도 함께 접근할 수 있는 것 같다. 
=> 이유를 이제 알았다. viewmodel은 보통 생명주기가 activity의 시작과 끝을 함께하는데 그것은 viewModel 내부에viewModelStoreOwner가 존재하고 이는 ComponentActivity, Fragment, NavBackStackEntry의 서브클래스이다. 즉, viewModelProvider가 viewModelStoreOwner를 통해 viewModel을 관리하고 접근.

즉, compose에서 comsable 단위로 viewmodel 생명주기를 사용할 수 없지만 navBackStackEntry를 활용해서 각각의 viewmodel을 사용하거나 nestedNavigation을 이용하여 일종의 sharedViewModel을 구현할 수도 있다.

ps. @HiltViewModel 어노테이션은 기본적으로 SaveStateHandle의 생성자 주입을 지원함.

```kotlin
@Composable
inline fun <reified T : ViewModel> NavBackStackEntry.sharedViewModel(navController: NavController): T {
    val navGraphRoute = destination.parent?.route ?: return viewModel()
    val parentEntry = remember(this) {
        navController.getBackStackEntry(navGraphRoute)
    }
    return viewModel(parentEntry)
}

@Composable
NavHost(
        modifier = modifier,
        navController = navController,
        startDestination = AuthScreen.Auth.route
    ) {
        navigation(
            startDestination = MainScreen.Home.route,
            route = MainScreen.Main.route
        ) {
            composable(MainScreen.Home.route) {
                val viewModel = it.sharedViewModel<HomeViewModel>(navController)
                HomeScreen(viewModel)
            }
            composable(MainScreen.MyPage.route) {
                val viewModel = it.sharedViewModel<HomeViewModel>(navController)
                DetailScreen(viewModel)
            }
    }
}
```
출처 : https://velog.io/@kej_ad/Android-Compsoe-Jetpack-Navigation-Nested-Graph%EC%99%80-Shared-ViewModel

hilt를 사용중이었다면 [공식문서](https://developer.android.com/develop/ui/compose/libraries?hl=ko#hilt-navigation)에 따라 
```kotlin
NavHost(navController, startDestination = startRoute) {
        navigation(startDestination = innerStartRoute, route = "Parent") {
            // ...
            composable("exampleWithRoute") { backStackEntry ->
                val parentEntry = remember(backStackEntry) {
                    navController.getBackStackEntry("Parent")
                }
                val parentViewModel = hiltViewModel<ParentViewModel>(parentEntry)
                ExampleWithRouteScreen(parentViewModel)
            }
        }
    }
```
parent Id를 통해 얻은 backStackEntry를 viewModelStoreOwner로 설정.

```kotlin
@Composable
inline fun <reified VM : ViewModel> hiltViewModel(
    viewModelStoreOwner: ViewModelStoreOwner = checkNotNull(LocalViewModelStoreOwner.current) {
        "No ViewModelStoreOwner was provided via LocalViewModelStoreOwner"
    },
    key: String? = null
): VM {
    val factory = createHiltViewModelFactory(viewModelStoreOwner)
    return viewModel(viewModelStoreOwner, key, factory = factory)
}
```

- - -
viewModelScope는 내부적으로 CloseableCoroutineScope 에 SupervisorJob()이 있음.
즉, child로만 예외 전파.