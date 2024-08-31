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


[kotlin 입출력](https://uknowblog.tistory.com/434)

StringTokenizer vs StreamTokenizer
전자가 편의성이 좋고 후자가 좀 더 빠르다고 함
후자 관련 [블로그](https://blog.naver.com/ohho7942/80054842596) 를 통해 StreamTokenizer는 nextToken() 호출 후 ttype, sval, nval 변수에서 값을 읽을 수 있음

with, run 스코프함수를 이용하면 메서드 자체로 접근가능 두 스코프 함수의 기능 차이는 없음