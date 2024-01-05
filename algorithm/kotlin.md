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

