[공식문서](https://kotlinlang.org/docs/sequences.html)
[참고 블로그](https://incheol-jung.gitbook.io/docs/study/undefined-2/6)
[참고 블로그2 iterator/generator](https://www.roach-dev.com/kotlin_coroutine_1/#%EB%93%A4%EC%96%B4%EA%B0%80%EA%B8%B0%EC%97%90-%EC%95%9E%EC%84%9C)

header.
파이썬에 yield 키워드에서 비슷한 개념을 처음 읽어보긴했었는데 당시에는 이해를 못했었음.
코틀린에서도 yield 키워드가 있음.
yield는 '양보하다' 뜻이 있음.
lazy 개념이 떠오른다.

- - - 
흔히 알고 있는 리스트, 배열 같은 자료구조. 자바와 코틀린에서는 컬렉션에 포함되어 있다.
컬렉션이 순회할 수 있는 이유는 iterable 하기 때문. 즉, iterator를 구현하고 있다.
직접 iterator를 구현한다면 생성, 순회 로직을 모두 작성하여야 한다.

generator는 순회, 생성 포함되어 있는 구조로 재진입이 가능한 loop로 설계되었다.
값을 생성하고 중단하고 재개할 수 있도록 yield 함수 부분이 중단 지점이 된다.

코틀린에서 generator는 sequence를 의미함. (컬렉션과 함께 코틀린 표준 라이브러리에 포함)
단순 컬렉션을 시퀀스로 만들기 위해서는 .asSequence를 이용.
동적으로 값을 생성할 때는 아래 방법
계산을 위한 함수에 기반한 시퀀스는 generateSequence(first ele) {}
	무한히 생성되나 함수에서 null 반환하면 생성 멈추고 유한하게 만들 수 있긴 함.
직접 sequence { yield(), yieldAll() } 빌더도 사용 가능.
시퀀스는 컬렉션과 다르게 요소마다 연산을 실행.

```kotlin
(1..100).asSequence().map { it * 4 }.filter { it > 15 }.first()
```
만약 sequence가 아니었다면 map과 filter 연산을 100번씩 수행한다. sequence는 각 요소마다 연산 실행.
이외에도 소수 찾기 알고리즘처럼 해당 수 제곱근까지 나누는 계산해야함으로 무한한 수를 생성하는 시퀀스를 통해 구현하면 유용.

즉, 
sequence를 사용하는 이유는 한 번에 값을 계산하지 않기 때문에 필요할 때만 계산하는 메모리 효율성.
대용량 데이터나 끝없는 데이터 스트림에서 데이터 처리 유용.


- - -
footer.
코루틴은 매커니즘이 비슷해보이나 제너레이터는 단방향 통신 코루틴은 양방향 통신
제너레이터의 개념인 생성 중단 재개의 매커니즘 기반 위에 확장된 코루틴.
코루틴 구현과 sequence, yield의 직접적인 관련은 없고 스택리스 방식으로 중단 시, 실행 중이던 스택 프레임 상태를 저장하고 복원하는 방법을 사용.