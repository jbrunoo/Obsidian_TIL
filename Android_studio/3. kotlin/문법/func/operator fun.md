operator란 ?
연산자 또는 작용소는 물리학과 수학에서 어떤 함수에 작용해 그 함수를 다른 함수로 변형시키는 함수를 말한다.
-위키백과 피셜


[공식문서](https://kotlinlang.org/docs/operator-overloading.html)
코틀린에도 여러 연산자가 있는데 기본적으로 산술, 대입, 단항 연산자 등이 있다.
operator fun을 활용하면 연산자를 오버로딩할 수 있다.


또한 invoke()도 호출 연산자이기 때문에 operator fun을 사용할 수 있다.


코드는 간소화 되지만 언제 어떻게 사용해야 할지는 규칙을 잘 정해보아야 할 것 같다.
