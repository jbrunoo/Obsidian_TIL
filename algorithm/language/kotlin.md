```kotlin
// 프로그래머스에서 같은 코드에 위 연산이 5~8ms, 아래 연산이 17~20ms
// why? inline 또는 filterIndexed 코드 작동 원리
// 아래 코드를 효율적이지 못하게 짰는지..
val odd = num_list.filterIndexed {i, _ -> i % 2 == 0 }.sum()
val even = num_list.filterIndexed {i, _ -> i % 2 != 0 }.sum()

// val odd = (num_list.indices step 2).sumOf { num_list[it] }
// val even = (1 until num_list.size step 2).sumOf { num_list[it] }
```

```kotlin
@SinceKotlin("1.2") public inline fun <T> Iterable<T>.sumOf(selector: (T) -> Int): Int { var sum = 0 for (element in this) { sum += selector(element) } return sum }
```
sumOf는 인라인 함수이나 selector 람다가 인라이닝 되지 않아서 발생할 것이라고 추측.

- - -

[kotlin 입출력](https://uknowblog.tistory.com/434)

StringTokenizer vs StreamTokenizer
전자가 편의성이 좋고 후자가 좀 더 빠르다고 함
후자 관련 [블로그](https://blog.naver.com/ohho7942/80054842596) 를 통해 StreamTokenizer는 nextToken() 호출 후 ttype, sval, nval 변수에서 값을 읽을 수 있음

with, run 스코프함수를 이용하면 메서드 자체로 접근가능 두 스코프 함수의 기능 차이는 없음

- - -
HashMap은 순서를 보장하지 못함. LinkedHashMap, mutableMap은 sort 후에도 보장

- - -
[참고 블로그](https://toonraon.tistory.com/56) [kotlin 나누기 vs 비트연산](https://jakewharton.com/which-is-better-on-android-divide-by-two-or-shift-by-one/)
이진탐색 구현 시, 자바 및 코틀린에서는 나누기와 비트 연산의 차이가 없다고 함.

- - -
얇은 복사 vs 깊은 복사
얇은 복사는 복사하는 대상의 주소를 참조. 즉, 원본과 복사한 객체는 값 변경 시 함께 변화.
깊은 복사는 값 전체가 복사.

mutable한 객체(primitive 타입이 아닌 경우, list, set, map 등)는 그대로 할당하면 얇은 복사가 일어남.
깊은 복사를 위해서 클래스에서 clone() 함수를 오버라이딩 하거나, data class를 쓸 수 있지만 이 또한 primitive 타입에만 가능. 
즉, 깊은 복사는 결국 객체를 새로 생성해주어야 함. 직렬, 역직렬 하는 방식도 있긴 함.
예로 list.toList()로 깊은 복사를 할 수 있는데 toList의 구현을 보면 iterable하게 다시 리스트를 만드는 것을 확인할 수 있다.
- - -
[코틀린 코드 실행 시간을 재보자](https://dev-sia.tistory.com/26)
`System.currentTimeMillis()`말고 `System.nanoTime()` 쓰자.
또는 java가 아닌 kotlin API를 사용해보자. 
```kotlin
import kotlin.system.measureNanoTime // Kotlin.time에 있음

val mt = measureNanoTime {
	// CODE
}
print(mt)

fun main() {
    val measuredTime = measureNanoTime {
        StreamTokenizer(System.`in`.bufferedReader()).run {
            fun i(): Int { nextToken(); return nval.toInt() }
            val n = i(); val m = i()
            val arr = Array(n) { IntArray(m) { i() } }
        }
    }
    print(measuredTime)


    val measuredTime2 = measureNanoTime {
        with(BufferedReader(InputStreamReader(System.`in`))) {
            val (n, m) = readLine().split(" ").map { it.toInt() }
            val arr = Array(n) { readLine().split(" ").map { it.toInt() }.toIntArray() }
        }
    }
    print(measuredTime2)
}
```

measureTime 같은 메서드 있는데 보면 kotlin.time.measureTime에 속해있다.
즉, 실행시간을 재는게 아닌 단순히 코드 시작 - 끝 시간을 비교하는 것.