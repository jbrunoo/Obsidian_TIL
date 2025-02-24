문자열이나 객체에 데이터를 navigate와 함께 전달했는데,

뒤로 돌아갈 때도 데이터를 전달할 수 있을까?

일단 popBackStack, navigateUp 메서드 자체에는 인수를 전달하는 기능이 없다.

뒤로 가기 수행 이전에 데이터를 전달하기 위해 기존의 navcontroller의 backStackEntry의 savedStateHandle을 통해 data를 set하고 읽어올 수 있다.


[[navigation에서 arguments 전달, savedStateHandle 관계]] - 해당 글에 적은 내용처럼 사용할 수 있는 이유는
hiltViewModel을 사용하면 viewModel에서 savedStateHandle을 기본 인자로 가지지만,
여기서 savedStateHandle은 해당 navigation backstackentry를 lifecycle owner로 삼기 때문에
기존의 previousBackStackEntry에 전달할 data를 set하고 currentBackStackEntry에서 data를 불러올 수 없다.

결론은 직접 데이터를 navHost 내 composable 안에서 직접 데이터를 옮겨주거나, sharedViewModel을 사용하는 수밖에 없을 것 같다.

일단은 postId 하나 전달하기 때문에 vm 대신 직접 전달할 예정이다.
깔끔하게 작성하기 위해 확장함수를 정의해주었다.

```kotlin

```