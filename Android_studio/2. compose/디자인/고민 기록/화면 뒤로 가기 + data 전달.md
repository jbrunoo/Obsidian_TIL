문자열이나 객체에 데이터를 navigate와 함께 전달했는데,

뒤로 돌아갈 때도 데이터를 전달할 수 있을까?
(신고/삭제 시 이전 화면으로 돌아가는데 api 호출이 이미되어있고 vm이 살아있어서 ViewModel.State만 업데이트 해주려고 함)

일단 popBackStack, navigateUp 메서드 자체에는 인수를 전달하는 기능이 없다.

뒤로 가기 수행 이전에 데이터를 전달하기 위해 기존의 navcontroller의 backStackEntry의 savedStateHandle을 통해 data를 set하고 읽어올 수 있다.


[[navigation에서 arguments 전달, savedStateHandle 관계]] - 해당 글에 적은 내용처럼 사용할 수 있는 이유는
hiltViewModel을 사용하면 viewModel에서 savedStateHandle을 기본 인자로 가지지만,
여기서 savedStateHandle은 해당 navigation backstackentry를 lifecycle owner로 삼기 때문에
기존의 previousBackStackEntry에 전달할 data를 set하고 currentBackStackEntry에서 data를 불러올 수 없다.

- 간단하게 말하면, **BackStackEntry.SavedStateHandle, ViewModel.SavedStateHandle**로 이름만 같고 다른 레벨인데 공유되는 줄 착각했다는 얘기다.

결론은 직접 데이터를 navHost 내 composable 안에서 직접 데이터를 옮겨주거나, sharedViewModel을 사용하는 수밖에 없을 것 같다.

일단은 postId 하나 전달하기 때문에 vm 대신 직접 전달할 예정이다.


- - -
ver 1.
깔끔하게 작성하기 위해 확장함수를 정의해주었다.

```kotlin
private enum class HideTargetScreen(val navigator: Any) {
    HOME_GALLERY(HomeGalleryNavigator),
    FEED(FeedNavigator),
}

/**
 *  직전 화면에 즉시 숨겨야 할 콘텐츠가 있는 경우, target ID를 직전 화면 스택에 저장 후 뒤로 가기
 *  save hideTargetContentId in previousBackStackEntry (HomeGallery || Feed) + popBackStack
 *
 *  ps. 직전 화면에서 NavController.getHideTargetContentId()를 통해 targetId를 획득 가능
 *
 * @param targetId
 */
fun NavController.navigateToBackWithHideTargetContentId(targetId: Long) {
    val prevDestination = this.previousBackStackEntry?.destination ?: return

    val hideTargetScreen = HideTargetScreen.entries.find { screen ->
        prevDestination.hasRoute(screen.navigator::class)
    }

    if (hideTargetScreen == null) {
        popBackStack()

        return
    }

    setHideTargetContentId(targetId)

    when (hideTargetScreen) {
        HideTargetScreen.HOME_GALLERY -> popBackStack<HomeGalleryNavigator>(inclusive = false)

        HideTargetScreen.FEED -> popBackStack<FeedNavigator>(inclusive = false)
    }
}

private fun NavController.setHideTargetContentId(targetId: Long) =
    this.previousBackStackEntry?.savedStateHandle?.set(AppConst.SavedStateHandle.UGC_HIDE_KEY, targetId)

fun NavController.getHideTargetContentId(): Long =
    this.currentBackStackEntry?.savedStateHandle?.get(AppConst.SavedStateHandle.UGC_HIDE_KEY) ?: -1L

```


```kotlin
navController.previousBackStackEntry?.savedStateHandle?.set("key", value) navController.popBackStack()
```
이렇게만 작성해도 되겠지만 다른 화면에서 사용되지 않도록 검사 진행 + string destination을 넣어주던 navigation에서 safe-type navigation으로 바꾼 후 동적으로 destination을 지정할 수 없다보니 길어졌다.


BackStackEntry에 붙으니 해당 루트에서 VM에 targetId를 넘겨주기 위해 이렇게 사용하게 되었다.
```kotlin
LifecycleEventEffect(Lifecycle.Event.ON_RESUME) {  
    val postId = getHideTargetContentId()  
  
    if (postId != -1L) {  
        feedViewModel.intent(Intent.HidePost(postId))  
    }  
}
```
navigation과 함께 쓰기 위한 최선 + 사용자 흐름에는 맞는 구현인 것 같으나 
UI 로직이 복잡해지는 것은 사실이다.

- - -
ver 2. 
현재는 SharedFlow를 통한 일회성 이벤트로 hideTargetId를 쏴주고 처리할 VM에서 받는 것으로 리펙토링 중이다.

