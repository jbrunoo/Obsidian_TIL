
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


단순히 누가 어떤 언어를 사용해서 개발했는지에 따라 위와 같이 차이점을 예전에 나누어 두었음..
다시 한 번 viewmodel에 대해 찾아보며 viewmodel을 보통 3가지 의미로 나누는 것 같다.

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


[MVVM with Grab Architecture](https://tv.naver.com/v/4637223?playlistNo=272653)
[깃헙예제](https://github1s.com/skydoves/DisneyCompose/blob/main/app/src/main/java/com/skydoves/disneycompose/ui/main/MainActivity.kt)
[코드랩](https://developer.android.com/codelabs/jetpack-compose-advanced-state-side-effects?hl=ko&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fcompose%3Fhl%3Dko%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fjetpack-compose-advanced-state-side-effects#4)
