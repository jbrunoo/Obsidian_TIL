머신러닝 분류기 그냥 함수로 사용하면 되지만.. 아키텍처 공부하면서 굳이 레이어 나누고 일반 클래스로 만들다가 hilt와 viewModel에서 주입이 잘못되었는지 머신러닝 모델을 쓰는 화면으로 넘어가는 버튼을 누르면 넘어가지 않고 메모리 용량이 계속 늘어나다 ANR 발생

주입 부분에서 잘못했을 수도 있지만 일단 머신러닝 모델도 앱에서 여러번 인스턴스로 생성할 것도 아닌데 바꿔보자 생각이 들었음. +room db를 hilt module 정의하는 부분에서 인사이트를 얻어 리팩토링 해보려고 함. 머신러닝 모델도 context 이용하여 모델 인스턴스를 불러오기 때문에 모델 불러오는 부분 hilt에 따로. 나머지 분류기 코드를 object로 만들면 되지 않을까 하는 일단 생각이 들었음.

작업 전, 이 참에 어렴풋이 알고 쓰던 object와 companion object부터 정의하기로 함.
[공식문서](https://kotlinlang.org/docs/object-declarations.html)
일단 object는 표현식(expression)과 선언식(declaration)이 있음
표현식은 일회성 사용을 위한 익명 클래스 or 익명 객체
```kotlin
val helloWorld = object {
    val hello = "Hello"
    val world = "World"
    // object expressions extend Any, so `override` is required on `toString()`
    override fun toString() = "$hello $world"
}

print(helloWorld)
```
상속받아 멤버 구현, 재정의도 가능하고 상위 유형에 생성자가 있으면 상위 유형에 매개변수 전달하면 됨.


