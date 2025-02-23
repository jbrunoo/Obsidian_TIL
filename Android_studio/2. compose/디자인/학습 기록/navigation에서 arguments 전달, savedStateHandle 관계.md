
1. 기존에는 route 문자열에 + {argument} 하여 navHost 내 composable에서 backStackEntry에서 값을 가져올 수 있었다.

2. 2.8.0 이후로 safe-Type navigation으로 객체에 데이터를 포함시켜 전달할 수 있었다. ex) toRoute<>

3. viewModel에서 lifecycleowner로 navBackStackEntry를 가지기 때문에 savedStateHandle을 이용해서도 값을 가져올 수 있다.

- - - 
이번에 처한 상황 - [같은 질문](https://slack-chats.kotlinlang.org/t/16932540/how-can-i-collect-observe-changes-from-savedstatehandle-in-a)

navigate 시, 인수를 전달하는게 아니라 popBackStack 과 같은 작업에서는 인수를 전달할 수 없다.
그래서 
```kotlin
// 기존 navController.currentBackStackEntry?.savedStateHandle

// 현재 코드
fun NavController.popBackGalleryAfterReport(postId: Int) {
    this.getBackStackEntry(HomeGalleryNavigator).savedStateHandle["reportedPostId"] = postId

    popBackStack<HomeGalleryNavigator>(inclusive = false)
}
```
이런 형태로 NavController의 savedStateHandle을 통해 인수를 넘겨주고

viewModel 내에서 아래와 같이 작업을 시도했다.
```kotlin
init {    
	...
    // navController::popBackGalleryAfterReport 호출 시  
    launch {  
        savedStateHandle.getStateFlow("reportedPostId", -1).collect { postId ->  
            deletePost(postId = postId)  
        }  
    }}
```


결론을 먼저 말하자면, viewModel 인스턴스가 자체적으로 고유한 savedStateHandle 가지는 것이며,
네이밍만 같은 다른 savedStateHandle이라 접근할 수 없는 것이다.

결국 navcontroller의 backStackEntry.savedStateHandle로 보낸 데이터는
`navController.previousBackStackEntry?.savedStateHandle?` 와 같이 접근하여
LaunchedEffect 쪽에서 vm 내 메서드를 호출하여 현재는 처리해야 할 것 같다.
