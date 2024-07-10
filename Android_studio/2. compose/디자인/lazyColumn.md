[BoxWithConstraints](https://foso.github.io/Jetpack-Compose-Playground/foundation/layout/boxwithconstraints/)
Box와 유사, composable에 사용 가능한 최소/최대 너비, 높이를 얻을 수 있음.


[subComposeLayout](https://developer.android.com/reference/kotlin/androidx/compose/ui/layout/package-summary#SubcomposeLayout(androidx.compose.ui.layout.SubcomposeLayoutState,androidx.compose.ui.Modifier,kotlin.Function2))
```
java.lang.IllegalStateException: Asking for intrinsic measurements of SubcomposeLayout layouts is not supported. This includes components that are built on top of SubcomposeLayout, such as lazy lists, BoxWithConstraints, TabRow, etc. To mitigate this:
- if intrinsic measurements are used to achieve 'match parent' sizing, consider replacing the parent of the component with a custom layout which controls the order in which children are measured, making intrinsic measurement not needed
- adding a size modifier to the component, in order to fast return the queried intrinsic measurement.
```

[Advanced Layout Concept](https://www.youtube.com/watch?v=l6rAoph5UgI&list=RDCMUCVHFbqXqoYvEWM1Ddxl0QDg&start_radio=1&t=4s)
[medium 글](https://medium.com/@SeopSeo/%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EB%82%B4-%EC%95%84%EC%9D%B4%ED%85%9C%EB%93%A4%EC%9D%98-%EB%86%92%EC%9D%B4%EB%A5%BC-%EC%B5%9C%EB%8C%80-%EC%95%84%EC%9D%B4%ED%85%9C-%EB%86%92%EC%9D%B4%EB%A1%9C-%EC%A7%80%EC%A0%95%ED%95%A0-%EC%88%98-%EC%9E%88%EC%9D%84%EA%B9%8C-d6cd89baa7c0)
compose에서도 constraintLayout을 사용가능한데 이것을 통해 해결한 [stackoverflow 답변](https://stackoverflow.com/questions/66711882/when-will-subcompose-layout-support-intrinsic-layout-if-ever)을 보았음

lazyColumn에서 IntrinsicSize 를 사용했다가 만난 에러
적혀 있듯, SubcomposeLayout, such as lazy lists, BoxWithConstraints, TabRow, etc.
lazy layouts이 효율적으로 동작하는 이유는 하위 컴포넌트의 크기를 계산하여 화면에 표시될 만큼만 로딩하기 때문인데
그 과정에서 subComposeLayout을 사용하기 때문에 IntrinsicSize와 충돌.

내부 컴포넌트에서 IntrinsicSize를 활용하거나 LocalConfiguration을 활용해보자.