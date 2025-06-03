gms는 리더보드, 인증, 광고 등을 위해 사용한 구글 라이브러리인데,
해당 메서드를 수행하기 위해 필요한 client들이 activity를 인자로 생성해야한다.
```kotlin
PlayGames.getGamesSignInClient(context as Activity)
PlayGames.getLeaderboardsClient(context as Activity)
```

프로젝트는 singleActivity에 compose를 사용중이기 때문에 compose-navigation으로 화면이 붙어있고,
화면의 viewModel owner는 navBackStackEntry가 된다.

초기에 activityScoped, ActivityContext를 활용하여 leaderBoard, Auth, AD를 위한 helper, manager 클래스들을 만들었으나 이를 singleton인 repository와 hiltviewmodel 범위인 viewmodel에서 사용할 수 없다.

activity에서 주입시켜 다 붙일 것이 아니라면 compose 에서 어떻게 사용하는게 좋을지 고민.
 - - - 

아무래도 activityScoped를 제거하고 LocalActivity.current를 활용해서 activity를 동적으로 받게끔 구현해서 viewModel에서 사용하는 것이 가장 문제될 소지가 없을 것 같다는 생각이다. 이것도 코드가 지저분하긴 하다.

현재 프로젝트가 singleActivity라는 것을 알고 있기 때문에 @Singleton이나 @ActivityRetainedScoped([viewModelScoped](https://developer.android.com/training/dependency-injection/hilt-jetpack?hl=ko#viewmodelscoped))를 사용해도 에러가 나지 않지만 (아마 안할 것 같지만) activity를 여러 개 생성해서 해당 클래스를 사용하면 다시 scope를 지정해주거나 주입을 제거할 수도 있을 것 같다.

일단은 주석 남겨두고 singleton으로 구현하기로 결정했다.
