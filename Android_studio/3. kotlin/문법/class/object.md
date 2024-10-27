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


- - -
java를 깊게 공부하진 않았지만 java에서 static 키워드가 있는데 클래스 내부에서 정적으로 변수나 함수를 정의할 때 사용한다. object, companion object를 디컴파일해보면 static 붙어 있는 것을 볼 수 있다.
물론 자바와 코틀린이 같진 않다. 코틀린의 경우 객체를 반환하기 때문.

java 생태계에서 static 사용을 지양하는데 그 이유는 메모리와 관련이 깊다.
정적으로 선언되기 때문에 즉, 생명주기가 프로그램과 함께 간다는 것.
또한, java 쪽 언어는 jvm에 의한 gc가 동작해서 메모리 해제 같은 처리를 따로 하지 않는데
프로그램이 종료될 때까지 gc에 잡히지 않게 된다.

즉, 상수나 유틸 함수 정도만 포함하고 무거운 객체를 정적으로 선언해두지 말자. gc가 메모리 해제할 수 없다.
ps. effective kotlin 8장에 나오는 것 같은데 다시 책을 읽어야겠다..

메모리 누수 관리 법에 관해서는 추후에 한번 더 정리해보아야겠다.
간단하게는
리소스, 의존관계 등 정적으로 유지하지말고, 사용하지 않는 객체를 null로 설정한다 (그래야 gc가 잡아서 메모리 해제)